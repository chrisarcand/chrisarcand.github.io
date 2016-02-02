---
layout: page
permalink: /projects/
title: Projects
tagline: 
tags: [projects]
modified: 9-9-2013
comments: false
image:
  feature: banners/cover13.jpg
  credit: 
  creditlink: 
---

<div class="project-container-outer shadow">
  <div class="project-container-inner">
    <h3>Opsicle <a href="http://badge.fury.io/rb/opsicle"><img class="pull-right" src="https://badge.fury.io/rb/opsicle.svg" alt="Gem Version" height="18"></a><span class="label label-success pull-right" style="margin-right: 10px;">Active</span></h3>
    <p>
      "A gem bringing the glory of OpsWorks to your command line", Opsicle uses the AWS SDK to provide a command line
      interface to Amazon's OpsWorks application management service. It features deployments, direct SSH access to your
      instances, an in-terminal stack monitor, updating custom Chef recipes and more.
    </p>
    <p><a href="https://github.com/sportngin/opsicle" target="_blank">See this project on GitHub</a></p>
  </div>
</div>

<div class="project-container-outer shadow">
  <div class="project-container-inner">
    <h3>Enum For What!<a href="http://badge.fury.io/rb/enum_for_what"><img class="pull-right" src="https://badge.fury.io/rb/enum_for_what.svg" alt="Gem Version" height="18"></a><span class="label label-success pull-right" style="margin-right: 10px;">Active</span></h3>
    <p>
      A gem extending ActiveRecord to enable native support of the ENUM column type in MySQL. Originally forked from
      eletronick/enum_column with several enhancements written and curated from the unenumerable forks of this gem (see
      what I did there?) - including support from Rails 3.2 up to 4.2.
    </p>
    <p><a href="https://github.com/sportngin/enum_for_what" target="_blank">See this project on GitHub</a></p>
  </div>
</div>

<div class="project-container-outer shadow">
  <div class="project-container-inner">
    <h3>Dug ("[D]amn yo[u], [G]mail!")<a href="http://badge.fury.io/rb/dug"><img class="pull-right" src="https://badge.fury.io/rb/dug.svg" alt="Gem Version" height="18"></a><span class="label label-success pull-right" style="margin-right: 10px;">Active</span></h3>
    <p>
      Dug is a simple, configurable gem to organize your GitHub notification
      emails in ways Gmail can't and in an easier-to-maintain way than large,
      slow Google Apps Scripts. It interacts with Google's Gmail API to do all
      the things that people usually do with Gmail filters (label by
      organization name, repository name) as well as parse GitHub's custom X
      headers to label your messages with things like "Mentioned by name",
      "Assigned to me", "Commented", etc.
    </p>
    <p><a href="https://github.com/chrisarcand/dug" target="_blank">See this project on GitHub</a></p>
  </div>
</div>

<div class="project-container-outer shadow">
  <div class="project-container-inner">
    <h3>UMN Classroom Finder <span class="label pull-right">Retired</span></h3>
    <figure class="pull-right">
      <a href="/images/projects/scraper.png">
        <img class="shadow" src="/images/projects/scraper_tb.png" alt="" />
      </a>
      <figcaption>Ye Olde Search UI</figcaption>
    </figure>
    <p>
      In February 2013 I worked with some computer science colleagues from the UofM to create a 'classroom finder' web
      application. It searched a database we filled every hour for any unscheduled classrooms on campus and presented
      the information in a sorted, meaningful way. Built mostly in Python, we used the Beautiful Soup library to scrape
      the University's classroom management site for classroom schedules, placed them in a SQLite database, and
      presented sorted information in a Flask frontend designed for mobile devices so you could whip out your smartphone
      on campus and find a place to study on the go. The result was a quick and easy way to find classrooms to get away
      from the <i>very</i> busy study commons, unlike the U's classroom management site which was clunky and didn't have
      a very good way to browse for open classrooms.
      <br><br>
      It worked quite well. Unfortunately, on 5/20/13 the University replaced their R25 scheduling system for a new one
      called Astra; the application is currently inoperable as it was not updated to scrape the new system properly.
      <br><br>
      The project was headed by my good friend <a href="http://www.brdyorn.com" target="_blank">Brady Ouren</a>.
      You can read more about the specifics of how we built it on <a href="http://www.brdyorn.com/2013/02/01/classroom-scraper.html" target="_blank">his blog post about the subject</a>.
    </p>
    <p><a href="https://github.com/ChrisArcand/classroom_finder" target="_blank">See this project on GitHub</a></p>
  </div>
</div>

<div class="project-container-outer shadow">
  <div class="project-container-inner">
    <h3>'KIX' to C++ Language Translator <span class="label label-info pull-right">Completed</span></h3>
    <figure class="pull-right">
      <a href="/images/projects/3081w_project.png">
        <img class="shadow" src="/images/projects/3081w_project_tb.png" alt="" />
      </a>
    </figure>
    <p>
      I implemented a language translator in a semester long project for my CSCI3081W class in Spring 2012 at the University of Minnesota.
      The complexity and sheer amount of work (many thousands of lines of code) makes it stand out as one of the more prominent projects that
      I completed at the U.
      <br><br>
      Written entirely in C++, the command line application takes code written in .KIX (a fictional, functional-like coding language) and
      returns the equivalent form in C++. It has a substantial number of unit tests using CxxTest and one of the requirements for the
      assignment was to write organized, consistently-formatted code across the entire project. It is built in five different modules:
      <ul>
        <li><b>The scanner</b>, which identifies tokens in the KIX language using regular expressions.</li>
        <li><b>The parser</b>, an implementation of a recursive descent parser with operator precedence by Eric Van Wyk which is based off of work in <a href="http://javascript.crockford.com/tdop/tdop.html" target="_blank">Douglas Crockford's "Top Down Operator Precedence", a chapter in the book "Beautiful Code".</a></li>
        <li><b>The AST constructor</b>, which builds an abstract syntax tree of defined C++ classes containing the parsed tokens. In essence,
          the resulting AST is the raw, generic construction of the program in a form that can be ported to any language that supports the proper semantics.</li>
        <li><b>The translator</b>, which finally uses the generic AST and outputs the proper C++ syntax.</li>
      </ul>
      It's a pretty neat project; you could write a different translator to port KIX (well, KIX admittedly is not as useful!) to any language
      you want without rewriting a whole lot. And that's good - because you have to do some pretty crazy (read: ugly) stuff in C++ to make it do functional
      programming like KIX does.
    </p>
    <span class="github-link muted"  style="text-decoration: line-through" target="_blank">See this project on GitHub</span><br>
    As it is a school project which fulfills class requirements, I will not put the project on GitHub - but if you'd like to see it for an
    example of my work, let me know!</a>
    </p>
  </div>
</div>

<div class="project-container-outer shadow">
  <div class="project-container-inner">
    <h3>ChrisArcand.com <span class="label label-success pull-right">Active</span></h3>
    <div class="pull-right">
      <figure>
        <a href="/images/projects/site.png">
          <img class="shadow" src="/images/projects/site_tb.png" alt="" />
        </a>
        <figcaption>Python/Flask</figcaption>
      </figure>
      <figure>
        <a href="/images/projects/site2.png">
          <img class="shadow" src="/images/projects/site2_tb.png" alt="" />
        </a>
        <figcaption>Node.js/Express</figcaption>
      </figure>
    </div>
    <p>
      I've rewritten this very site anywhere from 3 to 4 times; it used to be my little guinea pig
      for introducing myself to new languages/tools/servers/etc. 
      <br><br>
      One version used Python with the <a href="http://flask.pocoo.org/" target="_blank">Flask</a> microframework
      (before it was cool!), <a href="http://jinja.pocoo.org/" target="_blank">Jinja2</a>, <a
      href="http://www.mongodb.org/" target="_blank">MongoDB</a> (NoSQL), and a whole bunch of respective Python
      libraries (<a href="http://mongoengine.org/" target="_blank">MongoEngine</a>, <a
      href="https://github.com/wtforms/wtforms" target="_blank">WTForms</a>, etc).  Another used <a
      href="http://www.nodejs.org" target="_blank">Node.js</a> with <a href="http://www.expressjs.com"
      target="_blank">Express</a>, <a href="http://jsantell.github.io/poet/" target="_blank">Poet.js</a>, and <a
      href="http://paularmstrong.github.io/swig/" target="_blank">Swig</a>. It's current [and final] version moves on to
      Ruby with <a href="http://jekyllrb.com/" target="_blank">Jekyll</a>, <a href="http://kramdown.gettalong.org/"
      target="_blank">kramdown</a>, and a more or <a href="http://lesscss.org/" target="_blank"><i>Less</i></a> (get
      it?) organized set of front end assets using <a href="http://gruntjs.com/" target="_blank">Grunt.js</a>.
    </p>
    <p><a href="https://github.com/ChrisArcand/chrisarcand.github.io" target="_blank">See this project on GitHub</a></p>
  </div>
</div>
