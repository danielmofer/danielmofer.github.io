---
layout: default
title: For Business Reasons
parent: TryHackMe
grand_parent: Labs
---

# For Business Reasons

The default settings for HTML compression are incompatible with the HTML
produced by Jekyll (4.1.1 or earlier) for line numbers from highlighted code
-- both when using Kramdown code fences and when using Liquid highlight tags.

To avoid non-conforming HTML and unsatisfactory layout, HTML compression
can be turned off by using the following configuration option:

{% highlight yaml %}
nmap -Pn -n -p- --min-rate 5000 --open -sV -sC 10.10.52.183
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-06 10:08 EDT
Nmap scan report for 10.10.52.183
Host is up (0.051s latency).
Not shown: 65531 filtered ports, 3 closed ports
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-generator: WordPress 5.4.2
| http-robots.txt: 1 disallowed entry 
|_/wp-admin/
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: MilkCo Test/POC site &#8211; Just another WordPress site

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 42.81 seconds
{% endhighlight %}

When using Kramdown code fences, line numbers are turned on globally by the
following configuration option:

{% highlight yaml %}
2222222
nmap -Pn -n -p- --min-rate 5000 --open -sV -sC 10.10.52.183
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-06 10:08 EDT
Nmap scan report for 10.10.52.183
Host is up (0.051s latency).
Not shown: 65531 filtered ports, 3 closed ports
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-generator: WordPress 5.4.2
| http-robots.txt: 1 disallowed entry 
|_/wp-admin/
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: MilkCo Test/POC site &#8211; Just another WordPress site

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 42.81 seconds
{% endhighlight %}

Line numbers can then be suppressed locally using Liquid tags (_without_ the
`linenos` option) instead of fences:

{% highlight yaml %}
{% raw %}{% highlight some_language %}
Some code
{% endhighlight %}{% endraw %}
{% endhighlight %}

## Workarounds

To use HTML compression together with line numbers, all highlighted code
needs to be wrapped using one of the following workarounds.
(The variable name `some_var` can be changed to avoid clashes; it can also
be replaced by `code` -- but note that `code=code` cannot be removed).

### Code fences (three backticks)

{% highlight default %}
{% raw %}{% capture some_var %}
```some_language
Some code
```
{% endcapture %}
{% assign some_var = some_var | markdownify %}
{% include fix_linenos.html code=some_var %}{% endraw %}
{% endhighlight %}

### Liquid highlighting

{% highlight default %}
{% raw %}{% capture some_var %}
{% highlight some_language linenos %}
Some code
{% endhighlight %}
{% endcapture %}
{% include fix_linenos.html code=some_var %}{% endraw %}
{% endhighlight %}

_Credit:_ The original version of the above workaround was suggested by
Dmitry Hrabrov at
<https://github.com/penibelst/jekyll-compress-html/issues/71#issuecomment-188144901>.

## Examples

✅ Using code fences + workaround (will only show line numbers if enabled globally in `_config.yml`):

{% capture code_fence %}
```js
nmap -Pn -n -p- --min-rate 5000 --open -sV -sC 10.10.52.183
Host discovery disabled (-Pn). All addresses will be marked 'up' and scan times will be slower.
Starting Nmap 7.91 ( https://nmap.org ) at 2021-05-06 10:08 EDT
Nmap scan report for 10.10.52.183
Host is up (0.051s latency).
Not shown: 65531 filtered ports, 3 closed ports
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-generator: WordPress 5.4.2
| http-robots.txt: 1 disallowed entry 
|_/wp-admin/
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: MilkCo Test/POC site &#8211; Just another WordPress site

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 42.81 seconds
```
{% endcapture %}
{% assign code_fence = code_fence | markdownify %}
{% include fix_linenos.html code=code_fence %}

✅ Using liquid highlighting + workaround:

{% capture code %}
{% highlight ruby linenos %}
# Ruby code with syntax highlighting and fixed line numbers using Liquid
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
{% endhighlight %}
{% endcapture %}
{% include fix_linenos.html code=code %}
{% assign code = nil %}

❌ With the default configuration options, the following example illustrates
the **incorrect** formatting arising from the incompatibility of HTML compression
and the non-conforming HTML produced by Jekyll for line numbers:

{% highlight ruby linenos %}
# Ruby code with syntax highlighting and unfixed line numbers using Liquid
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
{% endhighlight %}
