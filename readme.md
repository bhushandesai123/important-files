<h2 style="color: blue;">Summary of Network Policies in Kubernetes</h2>

<h3 style="color: green;">Key Concepts</h3>
<ul>
    <li><strong style="color: orange;">Ingress and Egress Rules:</strong>
        <ul>
            <li><strong>Ingress</strong> rules control incoming traffic to Pods based on namespace, IP, and Pod labels.</li>
            <li><strong>Egress</strong> rules manage outgoing traffic similarly.</li>
        </ul>
    </li>

    <li><strong style="color: orange;">Database Security:</strong>
        <ul>
            <li>Use ingress rules in a NetworkPolicy to restrict access to sensitive databases.</li>
        </ul>
    </li>

    <li><strong style="color: orange;">Kubernetes Version:</strong>
        <ul>
            <li>Ensure you are using EKS version greater than 1.27 for enhanced features.</li>
        </ul>
    </li>
</ul>

<h3 style="color: green;">Example Setup</h3>
<ol>
    <li><strong>Pods Creation:</strong>
        <ul>
            <li>Created two Pods: one for <strong>httpd</strong> and another for <strong>Redis</strong>.</li>
            <li>Download Redis in the Redis Pod and use the httpd image for the other Pod.</li>
        </ul>
    </li>

    <li><strong>Network Policy YAML:</strong>
        <ul>
            <li>Define a NetworkPolicy that specifies ingress and egress rules based on Pod labels.</li>
        </ul>
    </li>

    <li><strong>Testing Connectivity:</strong>
        <ul>
            <li>Attempting to ping the Redis Pod from the httpd Pod will fail if the policy restricts it.</li>
            <li>Use <code style="color: red;">kubectl get pod -o wide</code> to retrieve Pod IPs.</li>
        </ul>
    </li>
</ol>

<h3 style="color: green;">Compatibility</h3>
<ul>
    <li>Network Policies work with:
        <ul style="color: blue;">
            <li>Calico</li>
            <li>Cilium</li>
            <li>Romana</li>
            <li>Weave Net</li>
        </ul>
    </li>
    <li><strong style="color: orange;">Not compatible with Flannel.</strong></li>
</ul>

<h2 style="color: blue;">Example Network Policy YAML</h2>

<pre style="background-color: #f0f0f0; padding: 10px;">
<code style="color: green;">
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: example-network-policy
spec:
  podSelector:
    matchLabels:
      app: redis
  policyTypes:
    - Ingress
    - Egress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: httpd
      ports:
        - protocol: TCP
          port: 6379
  egress:
    - to:
        - podSelector:
            matchLabels:
              app: httpd
      ports:
        - protocol: TCP
          port: 80
</code></pre>

<h3 style="color: green;">Conclusion</h3>

<p style="color: black;">Network Policies in Kubernetes allow for fine-grained control over traffic flow, enhancing security for applications like databases. By properly configuring ingress and egress rules, you can effectively manage communication between Pods while ensuring sensitive data remains protected.</p>
