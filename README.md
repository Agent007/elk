Elasticearch, Logstash, Kibana
===
Assuming Homebrew is installed, run the following in your Mac OS X terminal:

```
brew install elasticsearch
```

Start elastic search:

```
elasticsearch
```

In another terminal window/tab, let's post the example JSON document, which will be indexed by elasticsearch:

```
curl -XPOST http://localhost:9200/engineers/engineer -d '{"favoriteBeer": "Heineken", "name": "John Smith", "numCommits": 200, "os": "Mac OS X", "programmingLanguage": "Go"}'
curl -XPOST http://localhost:9200/engineers/engineer -d '{"favoriteBeer": "Heineken", "name": "James Bond", "numCommits": 7007, "os": "Mac OS X", "programmingLanguage": "Scala"}'
curl -XPOST http://localhost:9200/engineers/engineer -d '{"favoriteBeer": "Dos Equis", "name": "The Most Interesting Man in the World", "numCommits": 1, "os": "Mac OS X", "programmingLanguage": "Go"}'
curl -XPOST http://localhost:9200/engineers/engineer -d '{"favoriteBeer": "Guiness", "name": "Jeff Lam", "numCommits": 7007, "os": "Linux", "programmingLanguage": "Scala"}'
```

Similar JSON documents can be posted multiple times.

Engineer Trends:

* Most common programming languages?

```
curl http://localhost:9200/engineers/engineer/_search -d '{"aggs":{"all_interests":{"terms":{"field":"programmingLanguage"}}}}'
```

* Programming languages where engineers hack on Macs?

```
curl http://localhost:9200/engineers/engineer/_search -d '{
                                                            "query": {
                                                              "match": {
                                                                "os": "Mac OS X"
                                                              }
                                                            },
                                                            "aggs": {
                                                              "all_interests": {
                                                                "terms": {
                                                                  "field": "programmingLanguage"
                                                                }
                                                              }
                                                            }
                                                          }'
```

* What kinds of beer do Go developers drink?

```
curl http://localhost:9200/engineers/engineer/_search -d '{"query":{"match":{"programmingLanguage": "Go"}},"aggs":{"all_interests":{"terms":{"field": "favoriteBeer"}}}}'
```

Kibana provides a prettier web-based UI to run and see visualizations for the results of various queries like the ones above.
