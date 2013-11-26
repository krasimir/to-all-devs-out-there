# To all devs out there.

The purpose of the following document is to present best practices used in our company. Some of the things are mandatory, some of them not. However, it is good to follow them in the name of the team play and good project's architecture.

## Getting started

Every project should start with a new Git repository. Don't leave the project's code only on your machine or on the FTP server. Commit your changes as often as possible. The version control should not act as a file storage or backup utility. It really matters how often you upload changes. Every single moment, you or some of your colleagues, may need to roll back some modification. If there is one commit per day this is not possible. It's also important to add a meaningful message. Something like `fixing bug` or `updates` doesn't help a lot. It should be clear what you did. 

Git has really powerful branch management. It's a good idea to split your work into `master` and `development` branches. Of course if the project is small you don't need to do this, but in most of the projects, this is needed. Read about [GitFlow](http://nvie.com/posts/a-successful-git-branching-model/) and get ideas from there. Keep your master branch up-to-date, valid and working. At every moment, it should be possible to get the code from there and release it on the production server.

Create a `README.md` file. If you don't like the [Markdown](http://daringfireball.net/projects/markdown/) format use plain text. The really important thing is to get such a file. It should give enough information to every developer so he could setup the project and start working with it. Here are few things which you should put notes to:

  - What is this project all about
  - What are the project dependencies (libraries, frameworks etc ...)
  - Steps for setupping of the project
  - Steps for deploying the project
  - Tricky parts 
  - Your name

## Development

Our development stack is mostly based on [LAMP](http://en.wikipedia.org/wiki/LAMP_(software_bundle)) in the server-side and JavaScript/HTML/CSS in the front-end. We are not really bound to any technology, framework or library. Feel free to use whatever you think is right, but before to start integrating some giant multi-platform solution consider the following:

  - Check if your colleagues know something about the technology. Using something that only you understand is a [single point of failure](http://en.wikipedia.org/wiki/Single_point_of_failure). 
  - Make sure that the chosen framework or library is well documented and there are articles or tutorials which explain how it works
  - Consult (if necessary) with your colleagues. Some of them may have bad experience with the technology.
  - Make sure that the tools which you want to use are actually installed on the production server. It's really easy to setup something on your laptop, but we are not shipping your machine. At the end everything should work on the official server.

### Beating blank page syndrome

The blank page syndrome is typical for the writers, but also for the programmers. Very often you are sitting in front of an empty class wondering how to start. There are tons of materials about modular programming, good and well structured code, best practices and so on. You should read a lot and improve your skills over the years. The purpose of this reading is not to make you a better developer. It is trying to give you a guidelines. Before to start writing something - THINK. Even if the deadline is tomorrow you still have a chance to produce something good. Plan your work, analyze the given tasks and imagine how your application will look like. Visualize the different components and how they are going to collaborate. There is something which I call *brute-force programming*. You start typing line after line without to think about architecture, design patterns or composition. You implement feature after feature and it seems that you are doing a good job. However, this is wrong. Sooner or later your crappy code will getting buggy and will stop working. That's because it is not planned very well and the final product looks more like a spaghetti rather then a software. Here are few tips for beating the blank page syndrome:

  - Testing, testing, testing - you should write tests. It is not true that following [TDD](http://en.wikipedia.org/wiki/Test-driven_development) takes more time. The true is that it leads to a better software. Try it and you will see. Once you start coding something ask yourself **"How I'm going to test it?"**. This question is like a key which opens a lot of doors. It produces classes with only one responsibility and encourages the modular programming.
  - Avoid speculative generality - that's a code smell which you get when you write something just because you think you will need it in the future. And in most of the cases you never use it. Yes, it is difficult to find the balance, but try to keep the abstractions as less as possible. Implement only what you actually need.

### Back-end development

In most of the cases we rely on frameworks developed by other people. Some of them are good, some bad, but we should play by their rules. Having this in mind you should follow the coding style provided by the system. If you need to write something custom make sure that it doesn't break some of the existing functionalities. Write in the `README.md` file how your code should be setup, where is the configuration and what is the main application flow. 

Do not mix the back-end code with the front-end logic (in some of tools like WordPress this can not be avoided). Develop the things like that so there is another guy which will work on the JavaScript/HTML/CSS part. Code like this one is not a good idea:

	<?php
		$user = $framework->getCurrentUser();
	?>
	<div>
		<a href="#" onclick="javascript:login('<?php echo $user->permission; ?>')";	
	</div>

Avoid global functions or variables. Try to put the things in their own classes. Following this approach you will be able to write tests and of course you will have a better control over your own code.

Follow [DRY](http://en.wikipedia.org/wiki/Don't_repeat_yourself) and [single responsibility](http://en.wikipedia.org/wiki/Single_responsibility_principle) principles. They both are essential knowledge which you should have. It's not a secret that they lead to a solid testable architecture.

### Front-end development

In general, try to keep the JavaScript, HTML and CSS separately. Do not mix them, do not put js code into the HTML, do not use inline styling. It should be like that so if someone wants to change the look of the elements to make this in the CSS and not to dig into the markup. This also means that all these three languages have their responsibilities. It is not good to apply CSS rules with JavaScript or attach event handlers directly into the HTML tags. Especially nowadays, you could create a lot of things with pure CSS. Like for example, animations, positioning, visual effects.

It is a good practice to concatenate your JavaScript and CSS. It's good because the users will make only one request to fetch them. Of course you can't write everything into one single file. Consider the usage of a task runner like [Grunt](http://gruntjs.com/) for the JavaScript and some preprocessor for the CSS. The idea is to keep your modular approach but at the end produce only one minified file.

Keep the external libraries to minimum. There are tons of useful stuff out there, but please, don't use them all. Having a 90K library just to get the value of an input field is not a good idea. It's always good to have less dependencies. It's even better to build a dependency free application.

### Configuration

It's good to keep the configuration in one place and this place to be somewhere in the back-end. It's also good to have at least two configurations. One for a local development and one for the production server. Describe everything into the `README.md` file.

### Deployment

In the ideal case you should have an SSH access to the server, log in there and pull the latest changes from the Git repository. This means that the configurations should be nicely split and ignored.  If the project is small, don't have a SSH access or there is no Git installed you have to upload the changes via FTP. However, this is really old-fashion way of doing the things. If that happen, please make sure that the master branch in the repository contains absolutely the same files as those on the server. Again, describe the deployment process in the `README.md` file.

## Ethics

Be kind and polite with your colleagues and always remember that you work together. Play with them not against them. Try to put yourself in their position and don't judge them. Sooner or later you will be there too. 

Always answer on emails, skype or phone calls. The last one is not mandatory, but try to do it. The feedback is always helpful. Even a simple `Thanks.` will do the job.

We all have questions. Try to ask the right person and before to do it, search for the answer. You don't have to spend a whole day on that, but read the brief again. If it is something technical try finding the answer in Google.

Problems. We meet problems everyday. When you are in a position to report something, try to provide a solution and include it in your email. Saying `I can't do this!` brings just a stress for you, for the project manager and for your colleagues. Make your life easier. It is much better to discuss the solving of the problem, rather then the problem itself. Also, make sure that you explain the situation very well and you do this as earlier as possible. Don't wait for the deadline to say that you can't run the code on the client servers, because a PHP module is missing.

Be responsible and care about the products which you are working on. The code which you deploy is yours. That's what you are as a developer. Don't leave broken or crappy stuff in the repository. Don't leave comments like `// what the hell is this`.

## Summary

Don't take all the things above as rules. Think about them as tips, good practices or advices. We all know that not everything could be done, but it is nice to think that we are working like that :)
