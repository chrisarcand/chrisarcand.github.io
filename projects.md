---
layout: page
permalink: /projects/
title: Projects
tagline: 
tags: [projects]
modified: 9-9-2013
comments: false
image:
  feature: banners/cover9.jpg
  credit: 
  creditlink: 
---

<div class="project-container-outer shadow">
  <div class="project-container-inner">
    <h3>Opsicle <span class="label label-success">Active</span></h3>
    <p>
      A collaboration with <a href="http://www.brdyorn.com" target="_none">Brady Ouren</a>, this project aimed to be a web-based
      bucket built in Node.js containing all of your notes, tasks, and bookmarks in a taggable, indexable way with a RESTful API to
      access them. It was designed to be a platform and machine independent method to store resources which would hopefully never really be
      obsolete as you could built any new frontend to change your workflow in the future.
    </p>
    <a href="https://github.com/tippenein/hink" class="github-link" target="_blank">See this project on GitHub</a>
  </div>
</div>

<div class="project-container-outer shadow">
  <div class="project-container-inner">
    <h3>Hink <span class="label">Retired</span></h3>
    <p>
      A collaboration with <a href="http://www.brdyorn.com" target="_none">Brady Ouren</a>, this project aimed to be a web-based
      bucket built in Node.js containing all of your notes, tasks, and bookmarks in a taggable, indexable way with a RESTful API to
      access them. It was designed to be a platform and machine independent method to store resources which would hopefully never really be
      obsolete as you could built any new frontend to change your workflow in the future.
    </p>
    <a href="https://github.com/tippenein/hink" class="github-link" target="_blank">See this project on GitHub</a>
  </div>
</div>

<div class="project-container-outer shadow">
  <div class="project-container-inner">
    <h3>UMN Classroom Finder <span class="label">Retired</span></h3>
    <div class="project-tb-container shadow">
      <a class="fancybox" rel="group" href="/images/projects/scraper.png">
        <img src="/images/projects/scraper_tb.png" alt="" />
      </a>
    </div>
    <p>
      In February 2013 I worked with some computer science colleagues from the UofM to create a 'classroom finder' web application. It searched a database we filled every hour for any unscheduled classrooms on campus and presented the information in a sorted, meaningful way. Built mostly in Python, we used the Beautiful Soup library to scrape the University's classroom management site for classroom schedules, placed them in a SQLite database, and presented sorted information in a Flask frontend designed for mobile devices so you could whip out your smartphone on campus and find a place to study on the go. The result was a quick and easy way to find classrooms to get away from the <i>very</i> busy study commons, unlike the U's classroom management site which was clunky and didn't have a very good way to
      browse for open classrooms.
      <br><br>
      It worked quite well. Unfortunately, on 5/20/13 the University replaced their R25 scheduling system for a new one called Astra, and so the application is currently inoperable as it hasn't been developed yet to scrape the new system properly.
      <br><br>
      The project is headed by my good friend <a href="http://www.brdyorn.com" target="_blank">Brady Ouren</a>, and is hosted on his site <a href="http://brontasaur.us" target="_blank">brontasaur.us</a>. You can read more about the specifics of how we built it on <a href="http://www.brdyorn.com/post/classroom-scraper" target="_blank">his blog post about the subject</a>.
    </p>
    <a href="https://github.com/ChrisArcand/classroom_finder" class="github-link" target="_blank">See this project on GitHub</a>
  </div>
</div>

<div class="project-container-outer shadow">
  <div class="project-container-inner">
    <h3>'KIX' to C++ Language Translator <span class="label label-info">Completed</span></h3>
    <div class="project-tb-container shadow">
      <a class="fancybox" rel="group" href="/images/projects/3081w_project.png">
        <img src="/images/projects/3081w_project_tb.png" alt="" />
      </a>
    </div>
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
    example of my work, <a href="/contact">please let me know.</a>
    </p>
  </div>
</div>

<div class="project-container-outer shadow">
  <div class="project-container-inner">
    <h3>ChrisArcand.com <span class="label label-success">Active</span></h3>
    <div class="project-tb-container shadow">
      <a class="fancybox" rel="group" href="/images/projects/site2.png">
        <img src="/images/projects/site2_tb.png" alt="" />
      </a>
    </div>
    <p>
      I've rewritten this very site anywhere from 3 to 4 times; it used to be my little guinea pig
      for introducing myself to new languages/tools/servers/etc. 
      <br><br>
      One version used Python with the Flask microframework
      (before it was cool!), Jinja2, mongoDB (NoSQL), and a whole bunch of respective Python libraries (MongoEngine, WTForms, etc).
      Another used <a href="http://www.nodejs.org" target="_blank">Node.js</a> with <a href="http://www.expressjs.com" target="_blank">Express</a>,
      <a href="http://jsantell.github.io/poet/" target="_blank">Poet.js</a>, and
      <a href="http://paularmstrong.github.io/swig/" target="_blank">Swig</a>. It's current [and final] version
      moves on to Ruby with Jekyll, kramdown, and a more or <i>Less</i> (get it?) organized set of front end assets using Grunt.js.
    </p>
    <a href="https://github.com/ChrisArcand" class="github-link" target="_blank">See previous versions on GitHub</a>
  </div>
</div>

I've worked on quite a few projects for work and play; This list is just a small sampling of larger
open projects that I have contributed to. 
