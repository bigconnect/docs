# Load Balancers

Load Balancers are a fully-managed, highly available network load balancing service. Load balancers distribute traffic to groups of Cloud Servers, which decouples the overall health of a backend service from the health of a single server to ensure that your services stay online.

There are a number of benefits to adding a load balancer to your infrastructure:

* Using a load balancer as a gateway gives you the flexibility to change your backend infrastructure without affecting the availability of your services, enabling seamless horizontal scaling, rolling deployments, large architecture redesigns, and more.
* Sharing the processing workload among a group of servers rather than relying on a single server prevents any one machine from being overwhelmed by requests.

Load balancing services give you the benefits of load balancing without the burden of managing the operational complexities.

## Plans and Pricing

Load balancer performance depends on the load balancer’s size. Larger load balancers have more compute resources allocated to them, meaning they can handle more simultaneous connections and requests per second \(RPS\) than the smaller ones.

However, performance may vary depending on the load balancer’s workload. Using different protocols and package management settings will produce different results. Because of this, we cannot provide specific performance metrics for each load balancer size, and we strongly recommend that you run your own benchmarks to see what size works for your application’s specific needs.

| Type | Services | Targets | SSL Certificates | Traffic | Price |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Small | 5 | 25 | 10 | 20 TB | 6.13 €/month |
| Medium | 15 | 75 | 25 | 20 TB | 18.63 € / month |
| Large | 30 | 150 | 50 | 20 TB | 37.38 €/month |

Small Load Balancers are recommended for static websites and blogs with low to moderate traffic

Medium Load Balancers are recommended for transaction-driven sites or services with moderate traffic.

Large oad Balancers are recommended for busy sites, services, or endpoints where high performance is a must.

