Liveness and Readiness Probes :

Liveness The liveness endpoint returns a 200 status if the application is operational, or a 500 status if it's not. The liveness probe checks this endpoint and considers the app healthy only if it receives a 200 status. Any other response, or no response at all, triggers a container restart.

Readiness The readiness endpoint verifies if the application has successfully connected to all components necessary for serving traffic. It returns a 200 status only when all required connections and initializations are complete, and a 500 status otherwise. The readiness probe uses this endpoint to determine if a container is ready to accept traffic. If the probe receives anything other than a 200 status, or no response, it keeps the container out of service.

Initial Delay and Period The initial delay sets how long to wait before the first probe runs, while the period determines the frequency of subsequent probes. For example, if an app takes 20 seconds to start, set the liveness probe's initial delay to at least 20 seconds. This ensures the probe only begins checking after the application has had sufficient time to initialize:

livenessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 25
  periodSeconds: 5


For readiness probes, the initial delay is less critical. An unresponsive app during startup correctly indicates it's not yet ready for traffic.

readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  periodSeconds: 5


Usage Use liveness probes to detect and restart unhealthy containers. Use readiness probes to determine when a container is ready to start accepting traffic. Together, they ensure your application remains healthy and responsive in a Kubernetes environment.

Sample logs :

kubectl logs -f grade-submission-api-deployment-75867884b5-czsjx -n grade-submission

{"level":"info","message":"Grade service is running on port 3000","service":"grade-service"}
{"level":"info","message":"Readiness probe failed - application not ready yet","service":"grade-service"}
{"level":"info","message":"GET /readyz 503 3ms","meta":{"req":{"headers":{"accept":"*/*","connection":"close","host":"10.244.0.24:3000","user-agent":"kube-probe/1.37"},"httpVersion":"1.1","method":"GET","originalUrl":"/readyz","query":{},"url":"/readyz"},"res":{"statusCode":503},"responseTime":3},"service":"grade-service"}

{"level":"info","message":"Application is ready to serve requests","service":"grade-service"}
{"level":"info","message":"Readiness probe succeeded","service":"grade-service"}
{"level":"info","message":"GET /readyz 200 2ms","meta":{"req":{"headers":{"accept":"*/*","connection":"close","host":"10.244.0.24:3000","user-agent":"kube-probe/1.37"},"httpVersion":"1.1","method":"GET","originalUrl":"/readyz","query":{},"url":"/readyz"},"res":{"statusCode":200},"responseTime":2},"service":"grade-service"}

{"level":"info","message":"Readiness probe succeeded","service":"grade-service"}
{"level":"info","message":"GET /readyz 200 1ms","meta":{"req":{"headers":{"accept":"*/*","connection":"close","host":"10.244.0.24:3000","user-agent":"kube-probe/1.37"},"httpVersion":"1.1","method":"GET","originalUrl":"/readyz","query":{},"url":"/readyz"},"res":{"statusCode":200},"responseTime":1},"service":"grade-service"}

{"level":"info","message":"GET /healthz 200 1ms","meta":{"req":{"headers":{"accept":"*/*","connection":"close","host":"10.244.0.24:3000","user-agent":"kube-probe/1.37"},"httpVersion":"1.1","method":"GET","originalUrl":"/healthz","query":{},"url":"/healthz"},"res":{"statusCode":200},"responseTime":1},"service":"grade-service"}
{"level":"info","message":"Readiness probe succeeded","service":"grade-service"}