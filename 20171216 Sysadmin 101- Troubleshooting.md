Sysadmin 101: Troubleshooting
系统管理 101: 排错
======
I typically keep this blog strictly technical, keeping observations, opinions and the like to a minimum. But this, and the next few posts will be about basics and fundamentals for starting out in system administration/SRE/system engineer/sysops/devops-ops (whatever you want to call yourself) roles more generally.
Bear with me!

一般我会将这个博客的内容限定在严肃的技术，持续关注某个话题，个人观点以及自己的小爱好。相对以上几点而言，后面几篇将会更多关注系统管理/ SRE /系统工程师/系统操作/开发运维（不管你自己怎么称呼这些个吧）几个角色。


"My web site is slow"
“我的网页好慢啊”

I just picked the type of issue for this article at random, this can be applied to pretty much any sysadmin related troubleshooting. It's not about showing off the cleverest oneliners to find the most information. It's also not an exhaustive, step-by-step "flowchart" with the word "profit" in the last box. It's about general approach, by means of a few examples.
The example scenarios are solely for illustrative purposes. They sometimes have a basis in assumptions that doesn't apply to all cases all of the time, and I'm positive many readers will go "oh, but I think you will find…" at some point.
But that would be missing the point.

我只是为这篇文章随机的选了个典型的情况，这适用于大多数有关系统排错的情况。它不是能够一下子找到所有线索的最聪明的方法，也不是一种穷举所有可能性，按图索骥就能解决问题的方法。这是解决问题的通用方法。
这个例子的情况只仅仅为了说明类似情况而已。前提是这并不能一直适应于大多数情况，我乐观的以为亲爱的读者在某种程度上会觉得“哦，我觉得你会找到某些线索。。。”。

Having worked in support, or within a support organization for over a decade, there is one thing that strikes me time and time again and that made me write this;
**The instinctive reaction many techs have when facing a problem, is to start throwing potential solutions at it.**

干过技术支持，或支持某组织超过10年，作为老司机，有个强烈的念头冲击着我的内心，并驱使我写下这句话；
**面临问题时，会本能的冒出某种潜在的解决方案。**

"My website is slow"
“我的网站慢”

  * I'm going to try upping `MaxClients/MaxRequestWorkers/worker_connections`
  * I'm going to try to increase `innodb_buffer_pool_size/effective_cache_size`
  * I'm going to try to enable `mod_gzip` (true story, sadly)

  * 我要尝试提高连接数最大值 `MaxClients/MaxRequestWorkers/worker_connections`
  * 我要尝试提高数据库缓冲池大小 `innodb_buffer_pool_size/effective_cache_size`
  * 我要尝试打开压缩参数 `mod_gzip`




"I saw this issue once, and then it was because X. So I'm going to try to fix X again, it might work".
“我之前见过这个问题，是因为某原因 X 。所以我打算把这个问题 X 修掉， 它会好的。”

This wastes a lot of time, and leads you down a wild goose chase. In the dark. Wearing greased mittens.
InnoDB's buffer pool may well be at 100% utilization, but that's just because there are remnants of a large one-off report someone ran a while back in there. If there are no evictions, you've just wasted time.
这浪费了好多时间啊，简直就是在赶鹅。在黑暗中戴着油腻的手套。
InnoDB 的缓冲池也许使用率在　100%，也许那仅仅是一次大的读写后的现象，如果没有回收，你只是在浪费时间。

### Quick side-bar before we start
### 在我们开始之前

At this point, I should mention that while it's equally applicable to many roles, I'm writing this from a general support system adminstrator's point of view. In a mature, in-house organization or when working with larger, fully managed or "enterprise" customers, you'll typically have everything instrumented, measured, graphed, thresheld (not even word) and alerted on. Then your approach will often be rather different. We're going in blind here.

If you don't have that sort of thing at your disposal;

### Clarify and First look
###　把问题说明白和第一印象

Establish what the issue actually is. "Slow" can take many forms. Is it time to first byte? That's a whole different class of problem from poor Javascript loading and pulling down 15 MB of static assets on each page load. Is it slow, or just slower than it usually is? Two very different plans of attack!

Make sure you know what the issue reported/experienced actually is before you go off and do something. Finding the source of the problem is often difficult enough, without also having to find the problem itself.
That is the sysadmin equivalent of bringing a knife to a gunfight.


### Low hanging fruit / gimmies

You are allowed to look for a few usual suspects when you first log in to a suspect server. In fact, you should! I tend to fire off a smattering of commands whenever I log in to a server to just very quickly check a few things; Are we swapping (`free/vmstat`), are the disks busy (`top/iostat/iotop`), are we dropping packets (`netstat/proc/net/dev`), is there an undue amount of connections in an undue state (`netstat`), is something hogging the CPUs (`top`), is someone else on this server (`w/who`), any eye-catching messages in syslog and `dmesg`?

There's little point to carrying on if you have 2000 messages from your RAID controller about how unhappy it is with its write-through cache.

This doesn't have to take more than half a minute. If nothing catches your eye - continue.


### Reproduce
### 重现
If there indeed is a problem somewhere, and there's no low hanging fruit to be found;

如果的确某处存在着问题，并且你不能轻易就找到答案。

Take all steps you can to try and reproduce the problem. When you can reproduce, you can observe. **When you can observe, you can solve.** Ask the person reporting the issue what exact steps to take to reproduce the issue if it isn 't already obvious or covered by the first section.

重复所有的步骤，并重现你的问题。当你能重现时，你就能观测。**当你能观测时，你就能解决了。**　如果没有显而易见的原因，向别人求教并描述如何重现问题。

Now, for issues caused by solar flares and clients running exclusively on OS/2, it's not always feasible to reproduce. But your first port of call should be to at least try! In the very beginning, all you know is "X thinks their website is slow". For all you know at that point, they could be tethered to their GPRS mobile phone and applying Windows updates. Delving any deeper than we already have at that point is, again, a waste of time.

Attempt to reproduce!

重试去重现问题！

### Check the log!
### 检查日志！

It saddens me that I felt the need to include this. But I've seen escalations that ended mere minutes after someone ran `tail /var/log/..` Most *NIX tools these days are pretty good at logging. Anything blatantly wrong will manifest itself quite prominently in most application logs. Check it.

### Narrow down
### 缩小范围

If there are no obvious issues, but you can reproduce the reported problem, great. So, you know the website is slow. Now you've narrowed things down to: Browser rendering/bug, application code, DNS infrastructure, router, firewall, NICs (all eight+ involved), ethernet cables, load balancer, database, caching layer, session storage, web server software, application server, RAM, CPU, RAID card, disks.
Add a smattering of other potential culprits depending on the set-up. It could be the SAN, too. And don't forget about the hardware WAF! And.. you get my point.
如果没有明显的原因，但你能重现问题，很好。所以你知道了网站是慢。现在你已经把问题转变为：浏览器渲染问题/某种 bug，软件代码，域名解析，路由，防火墙，网卡，网线，负载均衡，数据库，缓存层，会话存储，网页服务器软件，应用软件服务器，内存，处理器，磁盘阵列卡，磁盘。加上其他潜在的依赖部署的导致的问题。也许是网络存储，别忘了硬件网页防火墙。。你懂得！

If the issue is time-to-first-byte you'll of course start applying known fixes to the webserver, that's the one responding slowly and what you know the most about, right? Wrong!
You go back to trying to reproduce the issue. Only this time, you try to eliminate as many potential sources of issues as possible.

You can eliminate the vast majority of potential culprits very easily: Can you reproduce the issue locally from the server(s)? Congratulations, you've just saved yourself having to try your fixes for BGP routing.
If you can't, try from another machine on the same network. If you can - at least you can move the firewall down your list of suspects, (but do keep a suspicious eye on that switch!)

Are all connections slow? Just because the server is a web server, doesn't mean you shouldn't try to reproduce with another type of service. [netcat][1] is very useful in these scenarios (but chances are your SSH connection would have been lagging this whole time, as a clue)! If that's also slow, you at least know you've most likely got a networking problem and can disregard the entire web stack and all its components. Start from the top again with this knowledge (do not collect $200). Work your way from the inside-out!

Even if you can reproduce locally - there's still a whole lot of "stuff" left. Let's remove a few more variables. Can you reproduce it with a flat-file? If `i_am_a_1kb_file.html` is slow, you know it's not your DB, caching layer or anything beyond the OS and the webserver itself.
Can you reproduce with an interpreted/executed `hello_world.(py|php|js|rb..)` file? If you can, you've narrowed things down considerably, and you can focus on just a handful of things. If `hello_world` is served instantly, you've still learned a lot! You know there aren't any blatant resource constraints, any full queues or stuck IPC calls anywhere. So it's something the application is doing or something it's communicating with.

Are all pages slow? Or just the ones loading the "Live scores feed" from a third party?

**What this boils down to is; What 's the smallest amount of "stuff" that you can involve, and still reproduce the issue?**

Our example is a slow web site, but this is equally applicable to almost any issue. Mail delivery? Can you deliver locally? To yourself? To <common provider here>? Test with small, plaintext messages. Work your way up to the 2MB campaign blast. STARTTLS and no STARTTLS. Work your way from the inside-out.

Each one of these steps takes mere seconds each, far quicker than implementing most "potential" fixes.

### Observe / isolate
### 观察 / 隔离

By now, you may already have stumbled across the problem by virtue of being unable to reproduce when you removed a particular component.
到此为止，你可能已经　当你移除掉某段特别的内容。
But if you haven't, or you still don't know **why** ; Once you've found a way to reproduce the issue with the smallest amount of "stuff" (technical term) between you and the issue, it's time to start isolating and observing.

但如果你还没有，或者还不知道 **为什么** ；一旦你已经发现了某个方法去重现问题所在，下一步就可以开始隔离和观测了。

Bear in mind that many services can be ran in the foreground, and/or have debugging enabled. For certain classes of issues, it is often hugely helpful to do this.

Here's also where your traditional armory comes into play. `strace`, `lsof`, `netstat`, `GDB`, `iotop`, `valgrind`, language profilers (cProfile, xdebug, ruby-prof…). Those types of tools.
这儿有些常见的分析利器。　`strace`, `lsof`, `netstat`, `GDB`, `iotop`, `valgrind`, 语言分析工具（cProfile, xdebug, ruby-prof…）

Once you've come this far, you rarely end up having to break out profilers or debugers though.

[`strace`][2] is often a very good place to start.
You might notice that the application is stuck on a particular `read()` call on a socket file descriptor connected to port 3306 somewhere. You'll know what to do.

Move on to MySQL and start from the top again. Low hanging fruit: “Waiting_for * lock”, deadlocks, max_connections.. Move on to: All queries? Only writes? Only certain tables? Only certain storage engines?…

You might notice that there's a `connect()` to an external API resource that takes five seconds to complete, or even times out. You'll know what to do.

You might notice that there are 1000 calls to `fstat()` and `open()` on the same couple of files as part of a circular dependency somewhere. You'll know what to do.

It might not be any of those particular things, but I promise you, you'll notice something.
说白了好像并没有什么特别的东西，但我保证你会注意到某些值得了解的东西。

If you're only going to take one thing from this section, let it be; learn to use `strace`! **Really** learn it, read the whole man page. Don 't even skip the HISTORY section. `man` each syscall you don't already know what it does. 98% of troubleshooting sessions ends with strace.

如果你只是想从这儿学到什么，没问题；学会使用 `strace`！　**是真正**　的学习哦，读完它的　man 文档。甚至它的历史部分也不要错过。`man` 每个你没有真正了解的系统调用。98%的系统排错会被 strace　所终结。


--------------------------------------------------------------------------------

via: http://northernmost.org/blog/troubleshooting-101/index.html

作者：[Erik Ljungstrom][a]
译者：[lujun9972](https://github.com/lujun9972)
校对：[校对者ID](https://github.com/校对者ID)

本文由 [LCTT](https://github.com/LCTT/TranslateProject) 原创编译，[Linux中国](https://linux.cn/) 荣誉推出

[a]:http://northernmost.org
[1]:http://nc110.sourceforge.net/
[2]:https://linux.die.net/man/1/strace

