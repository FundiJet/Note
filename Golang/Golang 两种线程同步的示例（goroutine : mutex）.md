# Golang 两种线程同步的示例（goroutine / mutex）

### 使用 goroutine 与 channel

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"time"
)

type Func func(key string) (interface{}, error)

type Memo struct {
	requests chan request
}

type request struct {
	key      string
	response chan<- result
}

type result struct {
	value interface{}
	err   error
}

type entry struct {
	res   result
	ready chan struct{} // closed when res is ready
}

func main() {
	test()
}

func (memo *Memo) Get(key string) (interface{}, error) {
	response := make(chan result)
	memo.requests <- request{key: key, response: response}
	res := <-response
	return res.value, res.err
}

func (memo *Memo) Close() { close(memo.requests) }

func (memo *Memo) server(f Func) {
	cache := make(map[string]*entry)
	for req := range memo.requests {
		e := cache[req.key]
		if e == nil {
			e = &entry{ready: make(chan struct{})}
			cache[req.key] = e
			go e.call(f, req.key)
		}
		go e.deliver(req.response)
	}
}

func (e *entry) call(f Func, key string) {
	e.res.value, e.res.err = f(key)
	close(e.ready)
}

func (e *entry) deliver(response chan<- result) {
	<-e.ready
	response <- e.res
}

func New(f Func) *Memo {
	memo := &Memo{make(chan request)}
	go memo.server(httpGetBody)
	return memo
}

func test() {
	m := New(httpGetBody)
	for _, url := range incomingURLs() {
		start := time.Now()
		value, err := m.Get(url)
		if err != nil {
			log.Print(err)
		}
		fmt.Printf("%s, %s, %d bytes\n",
			url, time.Since(start), len(value.([]byte)))
	}
	m.Close()
}

func incomingURLs() []string {
	return []string{
		"https://www.baidu.com",
		"https://music.163.com",
	}
}

func httpGetBody(url string) (interface{}, error) {
	resp, err := http.Get(url)
	if err != nil {
		return nil, err
	}
	defer resp.Body.Close()
	return ioutil.ReadAll(resp.Body)
}
```

------

### sync.Mutex

```go
package memo1

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"sync"
	"time"
)

type Memo struct {
	mu    sync.Mutex
	f     Func
	cache map[string]*entry
}

type Func func(key string) (interface{}, error)

type result struct {
	value interface{}
	err   error
}

type entry struct {
	ready chan struct{}
	res   result
}

func main() {
	test()
}

func (memo *Memo) Get(key string) (interface{}, error) {
	memo.mu.Lock()
	e := memo.cache[key]
	if e == nil {
		e = &entry{ready: make(chan struct{})}
		memo.cache[key] = e
		memo.mu.Unlock()

		e.res.value, e.res.err = memo.f(key)
		close(e.ready)
	} else {
		memo.mu.Unlock()
		<-e.ready
	}
	return e.res.value, e.res.err
}

func test() {
	m := New(httpGetBody)
	for _, url := range incomingURLs() {
		start := time.Now()
		value, err := m.Get(url)
		if err != nil {
			log.Print(err)
		}
		fmt.Printf("%s, %s, %d bytes\n",
			url, time.Since(start), len(value.([]byte)))
	}
}

func incomingURLs() []string {
	return []string{
		"https://www.baidu.com",
		"https://music.163.com",
	}
}

func httpGetBody(url string) (interface{}, error) {
	resp, err := http.Get(url)
	if err != nil {
		return nil, err
	}
	defer resp.Body.Close()
	return ioutil.ReadAll(resp.Body)
}

func New(f Func) *Memo {
	return &Memo{f: f, cache: make(map[string]*entry)}
}
```

