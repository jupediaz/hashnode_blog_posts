# Optimizing scalability and health of your Laravel APIs with monitoring tools like Datadog and New Relic

## Monitoring tools

**Monitoring tools** are essential for **tracking the performance and health** of your API and **identifying bottlenecks** or other issues that may be affecting its scalability.

With a monitoring tool, you can track key metrics such as response times, error rates, and resource usage, and get alerts when these metrics exceed certain thresholds. This can help you proactively identify and resolve issues before they become major problems.

There are many monitoring tools available, including [**Datadog**](https://www.datadoghq.com/) and [**New Relic**](https://newrelic.com/). In this post, we'll show you how to use these tools to monitor your API and provide examples of the configuration and code needed to set them up.

## Datadog

[![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671829104471/47aea190-b1ab-4d3d-b733-94d2f78a88d6.png align="left")](https://www.datadoghq.com/)

To use [**Datadog**](https://www.datadoghq.com/) with your API, you'll need to sign up for an account and install the **Datadog** agent on your server. The **Datadog** agent is a small program that runs on your server and collects data about your API's performance and resource usage.

To install the **Datadog** agent on your server, you'll need to follow the instructions provided by **Datadog**. Typically, this involves running a script to install the agent and configure it to send data to your **Datadog** account.

Once the **Datadog** agent is installed and configured, you can start tracking key metrics for your API. For example, you can use the `dd_url_tag` function to track the response times for specific routes:

```php
use Illuminate\Http\Request;

...

public function getUsers(Request $request)
{
    \DD::tag('url', '/users');
    ...
}
```

In this example, the `dd_url_tag` function adds a `url` tag with the value `/users` to the data collected by the **Datadog** agent. This allows you to track the response times for this route separately from other routes.

You can also use the **Datadog API** to track custom metrics for your API. For example, you can use the `increment` function to track the number of times a particular event occurs:

```php
use Illuminate\Http\Request;

...

public function getUsers(Request $request)
{
    \DD::increment('users.get');
    ...
}
```

In this example, the `increment` function increments a metric called `users.get` each time the `getUsers` method is called. This allows you to track the number of times this route has been accessed.

You can also use **Datadog** to set up alerts for specific metrics. For example, you can set up an alert to notify you when the response time for a particular route exceeds a certain threshold:

```php
use Illuminate\Http\Request;

...

public function getUsers(Request $request)
{
    \DD::tag('url', '/users');
    \DD::alert_when('url.response_time', '>', 500);
    ...
}
```

In this example, the `alert_when` function sets up an alert that will be triggered when the response time for the `/users` route exceeds 500 milliseconds. You can customize the alert conditions and thresholds to suit your needs.

Using a monitoring tool like **Datadog** can help you track the performance and health of your API and identify bottlenecks or other issues that may be affecting its scalability. By setting up custom metrics and alerts, you can proactively monitor your API and take action to resolve any issues before they become major problems.

## New Relic

[![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671829365720/29c2494e-38ac-4e3b-a363-15181cde8ed0.png align="left")](https://newrelic.com/)

[**New Relic**](https://newrelic.com/) is another popular monitoring tool that you can use with your API. To use **New Relic** with your API, you'll need to sign up for an account and install the **New Relic** agent on your server. The **New Relic** agent is similar to the **Datadog** agent in that it collects data about your API's performance and resource usage.

To install the **New Relic** agent, you'll need to follow the instructions provided by New Relic. Typically, this involves downloading the agent and running a script to install and configure it.

Once the **New Relic** agent is installed and configured, you can start tracking key metrics for your API. For example, you can use the `recordMetric` function to track the response times for specific routes:

```php
use Illuminate\Http\Request;

...

public function getUsers(Request $request)
{
    \NewRelic::recordMetric('Custom/users/get', $request->duration());
    ...
}
```

In this example, the `recordMetric` function records a custom metric called `Custom/users/get` with the value of the request duration. This allows you to track the response times for this route separately from other routes.

You can also use the **New Relic** API to track custom metrics for your API. For example, you can use the `recordEvent` function to track the number of times a particular event occurs:

```php
use Illuminate\Http\Request;

...

public function getUsers(Request $request)
{
    \NewRelic::recordEvent('users.get', ['count' => 1]);
    ...
}
```

In this example, the `recordEvent` function records an event called `users.get` with a count of 1 each time the `getUsers` method is called. This allows you to track the number of times this route has been accessed.

Like **Datadog**, **New Relic** also allows you to set up alerts for specific metrics. For example, you can set up an alert to notify you when the response time for a particular route exceeds a certain threshold:

```php
use Illuminate\Http\Request;

...

public function getUsers(Request $request)
{
    \NewRelic::recordMetric('Custom/users/get', $request->duration());
    \NewRelic::recordCustomEvent('users.get.alert', ['duration' => $request->duration()], ['duration' => 'ms']);
    ...
}
```

In this example, the `recordCustomEvent` function records a custom event called `users.get.alert` with the request duration as a parameter. You can then set up an alert in the New Relic dashboard to notify you when this event is triggered with a duration greater than a certain threshold.

Using a monitoring tool like **New Relic** can help you track the performance and health of your API and identify bottlenecks or other issues that may be affecting its scalability. By setting up custom metrics and alerts, you can proactively monitor your API and take action to resolve any issues before they become major problems.

## Health and performance

Using a monitoring tool like **Datadog** or **New Relic** can help you track the performance and health of your API and identify bottlenecks or other issues that may be affecting its scalability.

By setting up custom metrics and alerts, you can proactively monitor your API and take action to resolve any issues before they become major problems. With the built-in support for these and other monitoring tools, it's easy to set up and use monitoring in your Laravel API.

It's important to note that monitoring tools like **Datadog** and **New Relic** are just one piece of the puzzle when it comes to ensuring the scalability and reliability of your API.

Many other factors can affect the performance and scalability of your API, including:

* the design of your API
    
* the architecture of your server infrastructure
    
* the workload of your servers
    

To ensure the scalability and reliability of your API, it's important to consider all of these factors and take a healthful approach to optimization.

This might include things like optimizing your:

* database queries
    
* using caching to reduce the load on your servers
    
* using load balancers to distribute incoming requests across multiple servers
    

Another important factor to consider is monitoring the health and performance of your server infrastructure.

This might include things like monitoring CPU and memory usage, disk space, and network traffic. By monitoring these key metrics, you can identify issues that may be affecting the performance of your API and take action to resolve them.

Finally, it's also important to consider the workload of your servers and ensure that they are adequately sized and configured to handle the demands of your API. This might include things like using larger or more powerful servers, adding more servers to your infrastructure, or using auto-scaling to dynamically scale your infrastructure based on demand.

By considering all of these factors and taking a healthful approach to optimization, you can ensure that your API is scalable, reliable, and performs well for your users.