Distributed Unit Test
========

Distributed Unit Test

    1. Run tests on a cluster.

    2. Scales horizontally and bounded by longest running test.

    It is not a parallel test framework but distributed test mechanism wherein a google project named "swarming" is used to isolate the test run and its required dependencies/environment.

    Currently the framework is onboarded for hadoop, hbase and Apache kudu.


Architecture
------------

Distributed testing infrastructure has two components: the backend and the frontend.

Frontend
^^^^^^^^^^^^

    The frontends are language and test framework dependent. "grind" is the frontend for Java projects using Surefire and JUnit.

    Frontends are responsible for enumerating the set of tests in the project, determining the test dependencies, and packaging up each test as an independent task.

Backend
^^^^^^^^^^

    The backend is a shared resource, handles storing test dependencies, running test tasks, monitoring ongoing test runs.

    It reuses some distributed test building blocks developed by Chromium: Swarming and its Go rewrite Luci.

Backend Components:
^^^^^^^^^^^^^^^^^^^

  * .isolate file format, which specfies dependencies on a test case and how to run it.


  * isolate server which is a centralized content store for serving dependencies. 


  * cli utility to interact with isolate server.


  * beanstalkd for a work queue manager.


  * MySQL to store master node state.


  * S3 to store stdout/stderr of failed tests.

Setup
--------
1. isolate server

    We need to build and host https://github.com/luci/luci-go on a Google App Engine. App uses google cloud storage for content cache.

    Able to Build the go app but STUCK here in hosting the same to GAE.

    Luci-go server is used as content cache for .isolate file and all the test dependecies which is enumerated by "grind".


2. dist_test server

    http://172.22.78.233:8081

3. slave

    http://172.22.78.233:8081

4. beanstalkd

    172.22.78.233

5. MySQL

  172.22.78.233
  DB: dist_test
  DB User: disttestuser

References
------------
Blog:   http://blog.cloudera.com/blog/2016/05/quality-assurance-at-cloudera-distributed-unit-testing/

Presentation:   http://schd.ws/hosted_files/apachebigdata2016/67/distributed_testing_apache_big_data_2016.pptx

Isolate testing Framework:    https://www.chromium.org/developers/testing/isolated-testing/infrastructure

Swarming:   https://www.chromium.org/developers/testing/isolated-testing/for-swes

Dist Test:    https://github.com/cloudera/dist_test


Live Services
--------------
Google Isolate Server:    https://isolateserver.appspot.com

Cloudera Test Dashboard:  http://dist-test.cloudera.org

Cloudera Isolate Server:  http://isolate.cloudera.org:4242
