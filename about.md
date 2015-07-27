---
layout: page
title: About
permalink: /about/
key: 1437063393
---
<div class="about">
	<h2>INFO</h2>
	{% if site.user.email%}
	<p>
		<em>email</em> : <a href="mailto:{{ site.user.email }}">i@searchp.cc</a>
	</p>
	{% endif %}
	{% if site.user.weibo%}
	<p>
		<em>weibo</em> : <a href="{{ site.user.weibo }}">weibo.com/{{ site.username }}</a>
	</p>
	{% endif %}
	{% if site.user.twitter%}
	<p>
		<em>twitter</em> : <a href="{{ site.user.twitter }}">twitter.com/{{ site.username }}</a>
	</p>
	{% endif %}
	{% if site.user.github %}
	<p>
		<em>github</em> : <a href="{{ site.user.github}} ">github.com/{{ site.username }}</a>
	</p>
	{% endif %}

	{% if site.user.desc %}
		<h2>DESC</h2>
		<p>
			{{ site.user.desc }}
		</p>
	{% endif %}
	{% include extends/duoshuo.html %}
</div>
