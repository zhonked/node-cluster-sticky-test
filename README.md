# Node cluster sticky session test
Simple test server creating one master and one worker node via node.js cluster module

The pauseOnConnect flag was added recently to prevent performance issues for pass through of sockets from master to work, however there appears to be some issue with sockets/fds being closed before they are read or some other type of clobbering going on.

Based on code from: https://github.com/elad/node-cluster-socket.io

Original discussion thread with elad: https://github.com/elad/node-cluster-socket.io/issues/10

To start the test server:
 * npm install
 * node sticky-cluster-test.js

Run siege against the master node that proxies to the worker:
 * siege -v -d1 -c100 -r10 http://localhost:3000
 * see failed transactions

``` 
Transactions:                914 hits
Availability:              91.40 %
Elapsed time:               9.08 secs
Data transferred:           0.01 MB
Response time:              0.01 secs
Transaction rate:         100.66 trans/sec
Throughput:             0.00 MB/sec
Concurrency:                0.67
Successful transactions:         914
Failed transactions:              86
Longest transaction:            0.04
Shortest transaction:           0.00
```

Run siege directly against the worker node:
 * siege -v -d1 -c100 -r10 http://localhost:4001
 * no more failed transactions
```
Transactions:               1000 hits
Availability:             100.00 %
Elapsed time:              10.05 secs
Data transferred:           0.01 MB
Response time:              0.01 secs
Transaction rate:          99.50 trans/sec
Throughput:             0.00 MB/sec
Concurrency:                0.90
Successful transactions:        1000
Failed transactions:               0
Longest transaction:            0.07
Shortest transaction:           0.00
```

