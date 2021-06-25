---
layout: post
title:  "[threads]: Implementing Async callbacks in C++"
date:   2021-06-25 10:54:00 +0530
categories: threads
---

Recently, I'd written a threadpool library, [simplethreadpool][github-link], in C++, which allows you to start few threads 
and facilates assignment of tasks to threads whenever required. This saves the thread-creation overhead and is pretty easy to use.
Most of the time, you want to execute a callback after the job is done.
 For example,
After sorting a list of integers, send those integers over the network.

{% highlight CPP %}
addJob(sortIntegers, sendDataOverNetwork);
{% endhighlight %}

Problem here is that it's not advisible to wait in main thread.
I think it's best to use another thread for executing `sendDataOverNetwork()`. 
Using [simplethreadpool][github-link], it can be easily achieved.

{% highlight CPP %}
void sendDataOverNetwork(nonstd::result &res)
{
   //wait for integers to get sorted
   res.get();

   // send the data over network
   ...
}

void addJob(std::function<void()> f, std::function<void()> post_f, nonstd::result &res)
{
   pool.addJob(f, res);
   pool.addJob(std::bind(post_f, std::ref(res));
}

int main()
{
   nonstd::thread_pool pool(4);

   pool.start();

   nonstd::result res;
   addJob(sortIntegers, sendDataOverNetwork, res);

   while (1)
   {
      std::this_thread::yield();
   }

   pool.stop();
}
{% endhighlight %}

Refer to [this][example-link] example for more details.


[github-link]:https://github.com/amitesh-singh/simplethreadpool
[example-link]:https://github.com/amitesh-singh/simplethreadpool/blob/main/tests/async_callback.cpp
