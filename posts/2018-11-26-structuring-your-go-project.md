---
Title: Structuring your Go web project
PublishedDate: 2018-11-26
---
I was working on the Blog Engine [(Bla)](https://github.com/jacobkania/bla) as one of my first big projects written in Go. I did a bit of planning, then started organizing code into a way that I felt made sense. It quickly fell into chaos, since I was using certain OO design patterns that don't make any sense in a language like Go. This happened in different ways on two rewrites of the project -- until I finally had to make a switch in implementation, and it blew half the project apart.

I decided to rewrite the entire project from the ground-up, and to take plenty of time to make sure everything is designed in a flexible and meaningful way. I looked to open-source projects for inspiration, watched lots of talks online, researched through documentation, and found an exceptional [article](https://medium.com/statuscode/how-i-write-go-http-services-after-seven-years-37c208122831) by Go veteran Mat Ryer detailing his experience. Here is the culmination of lessons learned from these sources and my own experience:

# Free your main() function

I generally would recommend putting very little in your main function. If the application is going to get bigger, it will get totally unmanageable with any logic in `main()`. Instead, find a sequence of things that need to happen, and put all of the logic for those things in packages that they're related to. Use the `main()` function for gathering dependencies that will then be shared throughout the application.

In my example, I know at the highest level that my application needs to:

* Load a configuration file
* Create a database and tables if they don't exist, and prompt to create an admin user if one doesn't exist
* Initialize the HTTP/HTTPS servers
* Start the server

So my `main()` function does those 4 things, and nothing else.

# Pass dependencies around your application by reference

Dependencies should be created at the highest level that they'll be needed, then passed down through your application. In Bla, for example, the database, router, and configuration settings are used in creating the server, which will then pass the dependencies to where they're needed. So I create them all in the `main()` function, and pass them to the server when I create it there. It looks a bit like this:

```go
func main() {
	config := configuration.Load()

	router := httprouter.New()

	db, err := sql.Open("sqlite3", "./content/data/bla.db")
	if err != nil {
		log.Fatalf("Database failed to open")
	}

	err = Initialize(db)
	if err != nil {
		log.Fatalf("Failed to run initialization")
	}

	srv := server.Server{
		Config: config,
		Router: router,
		Db:     db,
	}

	log.Fatal(srv.Run())
}
```

This way, if I ever need to change the database implementation to be Postgres, for example, I can just swap it in this one place. Since I pass this instance of the database throughout the application by reference to a `sql.DB` interface, any implementation of `sql.DB` can be used in place of this `sqlite3` implementation. The whole application will work the same if I just change that one line to use a postgres driver. This is the Server struct which makes this possible:

```go
type Server struct {
	Config      *configuration.Configuration
	Router      *httprouter.Router
	Db          *sql.DB
	httpsServer *http.Server
	httpServer  *http.Server
}
```

# Keep all of your request handlers close to your server implementation

In Bla, the method in the last example, `srv.Run()`, does a few different things. First, it sets up all of the routes for the router that's given as a dependency. Here's the `Run()` method:

```go
func (s *Server) Run() error {
	s.setRoutes()

	httpUrl := ":" + strconv.Itoa(s.Config.HttpPort)
	httpsUrl := ":" + strconv.Itoa(s.Config.HttpsPort)
	s.newServer(httpsUrl, httpUrl)

	go s.httpServer.ListenAndServe()
	return s.httpsServer.ListenAndServeTLS(s.Config.CertFile, s.Config.KeyFile)
}
```

That's simple enough. Within `Run()` we have `setRoutes()`, which looks a little like this:

```go
func (s *Server) setRoutes() {
	...
	s.Router.GET("/post/id/:id", handleGetPostById(s.Db))
	s.Router.POST("/post", handleCreatePost(s.Db))
	s.Router.PUT("/post/id/:id", handleUpdatePost(s.Db))
	s.Router.DELETE("/post/id/:id", handleDeletePost(s.Db))
	...
}
```

So what it's doing here is using the shared dependency called `Db` that was given to the Server instance when we created it. We then pass that dependency into the handler functions. Keeping the handlers close to the server allows us to easily pass these dependencies to accomplish the goal of passing dependencies around by reference.

# Conclusion

The main idea that I've come to realize is that, to quote Rob Pike, *simplicity is complicated*. If you want to make an application in any language that is simple to work on, and simple to change, it takes a lot of planning and thought. It probably takes some re-writing. The best way to learn what makes a simple application is by doing, and sometimes you may need to change a lot of things along the way. It's hard work, but the end result is a code-base that is enjoyable to work with, and I think that is the greatest reward for your hard work.