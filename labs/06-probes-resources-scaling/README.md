# Lab 06: Probes, Resources, and Autoscaling (planned)

**Goal:** make your app self-healing and scalable.

Planned content:
- Liveness vs readiness vs startup probes, and what happens when each fails
- Requests vs limits, QoS classes, and how they affect scheduling and OOMKills
- Metrics Server and `kubectl top`
- HorizontalPodAutoscaler on CPU, then a load test to watch it scale

Bridge to AWS: ECS service auto scaling and ALB health checks are the same ideas.
