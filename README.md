# pool - A generic resource pool for go (golang)

### Example Usage

```go
package main

import "pool"

func init(){
	resourceOpen := func() (interface{}, error) {
		db, err := resourceNew()
		return db, err
	}
	resourceClose := func(r interface{}) error {
		db := resource.(*sql.DB)
		return db.resourceDel() 
	}
	err = NewResourcePool("db1", 10, 20, resourceOpen, resourceClose)
}

func main() {
	rp := pool.Name("db1")
	resource, err := rp.Get() // obtain a resource
	// use resource ... db what ever
	rp.Release(resource) // return resource to the pool
	...
	rp.Drain() // free up all resources and close the pool
}
```

### Features

- Simple interface
- Lazy creation of resources (create resources when needed, but keep a min on hand)

### Note
-  db.resourceDel() "as in db.Close()" must return errors just as go sql databases do

### ToDo
- Find a better way to handle errors when closing resources