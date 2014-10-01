# Notes

Notes for the following talks:

* [What Is a CSS Framework Anyway?](https://vimeo.com/95734680): Harry Roberts, Industry Conf
* [CSS Is a Mess](https://vimeo.com/99877232): Jonathan Snook, Beyond Tellerand
* [Slaying the Dragon: How to Refactor CSS for Maintainability](https://vimeo.com/100501790): Alicia Liu, Front-Trends
* [CSS in Your Pocket - Mobile CSS Tips from the Trenches](https://www.youtube.com/watch?v=vBHt61yDO9U&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA): Angelina Fabbro, CSSConf.US
* [Play Nice With CSS Tools and Methodologies](https://www.youtube.com/watch?v=-bZSTMLqf8Q&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA): Brad Westfall, HTML5DevConf
* [When Bootstrap Attacks](https://www.youtube.com/watch?v=xbpnqbM6cRk&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA): Pamela Fox, CSSConf.US
* [Automated CSS Testing](https://www.youtube.com/watch?v=2PU6JX4S7zI&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA): Jakob Mattsson, JSConf.Asia
* [Automating the Removal of Unused CSS](https://www.youtube.com/watch?v=833xr1MyE30&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA): Addy Osmani, Velocity Europe Conference
* [Architecting Scalable CSS](https://vimeo.com/70041549): Harry Roberts, Beyond Tellerand

Please add your own notes when you watch these or any other talks.

**Table of contents will go here**


## Mental framework

[Play Nice With CSS Tools and Methodologies](https://www.youtube.com/watch?v=-bZSTMLqf8Q&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA) discussed a good framework for thinking about the topics presented in the talks. I'd recommend watching that one early on just to have some clear categories for the other talks. 

He broke all discussions down into 4 categories:

1. Patterns
2. Implementation
3. Ideology
4. Methodology

In general I didn't take notes on implementation since it's likely to change, it's (fairly) easy to Google, and it's only applicable in certain cases.


##  CSS class naming

This topic seemed to come up the most. There were differing opinions about how to define "clear," some of which are mentioned below. No matter how you decide to define "clear" it should be consistent and the entire team should subscribe to the same method.

Some people think that class names should deal with the content of an element (e.g. `.book-title`). This can have problems when the code is literal (`.new-roman-title`) and then something changes (`.new-roman-title` now needs to be in Helvetica).

Others think the class names should only handle what the content looks like (e.g. `.large-text`). (Hint: Twitter-Bootstrap is, by necessity, this option. It doesn't know what your content is.) This can lead to lots of classes (`.small.box.border.rounded-coners`).


### Names should clarify intent

Don't tie the name to how the element looks (`.red-alert` vs. `.alert`) - what happens if the color changes?

Don't use too many class names (`.btn .big-btn .click-me` vs `.action-btn`). See: [use mixins]((#use-mixins)) or do the reverse...


### Namespace your class names

#### 1. Handy for search and replace

If you ever have to do a global find and replace, it'll be much easier if your class names are specific (`.title` vs. `.book-title`). Also, CSS is faster at rendering un-nested, specific class names than for many nested ones (`.book .title` vs `.book-title`).


#### 2. By component

@Lucy not sure what you're meaning here and how it's different to the section above.

It's nice to namespace your class names to the component they're for (`.heading` vs. `.profile-heading`). It will help keep similar items together, as well as letting you know where the class is used. This can also be useful for new features that may not go live - you'll be able to tell which CSS is relevant and delete it if needed.

**Examples:**

`.profile-heading` is good vs. `.profile` is bad

`.profile-heading` is good vs. `.profile` is bad

`.profile-heading` is good vs. `.profile` is bad


#### 3. Use data attributes for JavaScript

Use data attributes for JS so you're not mixing with CSS. As it [turns out](http://stackoverflow.com/questions/9181526/jquery-performance-select-by-data-attr-or-by-class) data attributes have about the same speed as class names.


#### During refactors

Namespace your old selectors with `-old`. (You could namespace your new selectors with `-new`, but then you'll need to update them all when you remove the old styles.) This has advantages:

1. you know you're not going to conflict with existing styles,
2. you can do a global find and replace to remove the old styles, and
3. it's clear to people not participating in your refactor which rules will stick around.


### Tools:

* [Object Oriented CSS](https://github.com/stubbornella/oocss/wiki)
* [BEM](http://bem.info/method/) and [thinking about BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)


### Talks:

* [When Bootstrap Attacks](https://www.youtube.com/watch?v=xbpnqbM6cRk&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA)
* [Slaying the Dragon: How to Refactor CSS for Maintainability](https://vimeo.com/100501790)
* [Play Nice With CSS Tools and Methodologies](https://www.youtube.com/watch?v=-bZSTMLqf8Q&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA)
* [CSS Is a Mess](https://vimeo.com/99877232)
* [Architecting Scalable CSS](https://vimeo.com/70041549)


## Remove unused CSS

The CSS file is larger if you have CSS that you're not using, and your site loads slower. It's best practice to remove rules that the site doesn't need.

However, it can be pretty difficult to know which CSS rules are absolutely not used. There are a few tools for this, listed below. You can also try adding a postfix for any existing class names you don't think are used (something like `-to-delete`). Then go over your site, see if anything broke, and find which of the class names have the postfix and are on that page. Of course, this is easier if you have visual regression tests to go over your site to find broken elements.

It's a good idea to check your site periodically so you only have to remove a few rules at a time rather than a bunch all in one go.

Often these rules are from bootstrap or the like, so they can be hard to remove on your own. Think about using mixins on your own classes rather than including everything from bootstrap.


### Tools:

* [uncss](https://github.com/giakki/uncss)
* The Chrome Developer Tools Audit tab
* You can use a grunt task to run over your site and only write the used CSS rules.


### Talks:

* [Automating the Removal of Unused CSS](https://www.youtube.com/watch?v=833xr1MyE30&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA)


## Use Mixins or Extends

If you're using a preprocessor you can use mixins or extends to:

* minimize duplication of code,
* only write the rules for selectors you're using (rather than bulk importing something like Bootstrap),
* can help keep class names descriptive (although that's also up to you!), and
* take care of vendor prefixing

Yes, using Mixins or Extends might result in a little extra CSS, but that's probably worth it to have cleaner Sass/Less/etc.

### Tools:

* [Compass](http://compass-style.org/)
* [Bourbon](http://bourbon.io/)
* Mixins provided by Bootstrap
    * [Why you should](http://ruby.bvision.com/blog/please-stop-embedding-bootstrap-classes-in-your-html)
    * [StackoverFlow about Less](http://stackoverflow.com/a/18855051/863846)
    * [SCSS mixins](https://github.com/anjlab/bootstrap-rails/blob/master/app/assets/stylesheets/twitter/bootstrap/_mixins.scss)

### Talks:

* [When Bootstrap Attacks](https://www.youtube.com/watch?v=xbpnqbM6cRk&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA)
* [Slaying the Dragon: How to Refactor CSS for Maintainability](https://vimeo.com/100501790)
* [Play Nice With CSS Tools and Methodologies](https://www.youtube.com/watch?v=-bZSTMLqf8Q&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA)
* [CSS Is a Mess](https://vimeo.com/99877232)


## Group code / modules logically

A future developer is going to make changes. How can you make their life as easy as possible? (Reminder: This future developer is likely to be you.)

Think of your site in pieces (modules); how you can reuse these modules?

* Group code for the same module together.
* Keep each module free standing (shouldn't need to have deeply nested / specific selectors).
* Possibly have similar class names for the elements in the module.

Think through how you can best communicate to the future developer what things go together and any conventions you're following.


### Tools:

* [SMACSS](http://smacss.com/)
* [shame.css](http://csswizardry.com/2013/04/shame-css/) group all your terrible hacks together (basically a "to-be-refactored" file).
* [pattern lab](http://patternlab.io/)


### Talks:
* [Architecting Scalable CSS](https://vimeo.com/70041549).
* [CSS Is a Mess](https://vimeo.com/99877232)


## Visual regression tests

Use visual regression testing to find any changes to the display of the site after CSS changes. Visual regression tests are generally quite brittle (you might not care about that 1px shift, but it will register as a test fail). But having them around might make you more secure when you refactor any CSS.

This isn't industry standard yet, or even an agreed "good practice" but it might make your life easier, so it's probably worth looking into.


### Tools:

* [Huxley](https://github.com/facebook/huxley)
* [Needle](https://github.com/bfirsh/needle)
* [PhantomCSS](https://github.com/Huddle/PhantomCSS)
* [Browser Stack](http://www.browserstack.com/) to see your site in different browsers
* I couldn't find a good blog post about the pros and cons of visual regression testing, but I'm sure it's not as clear as "you should do it"


### Talks:

* [Automated CSS Testing](https://www.youtube.com/watch?v=2PU6JX4S7zI&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA)
* [When Bootstrap Attacks](https://www.youtube.com/watch?v=xbpnqbM6cRk&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA)
* [Slaying the Dragon: How to Refactor CSS for Maintainability](https://vimeo.com/100501790)


## Responsive CSS

It's must easier to make the CSS responsive from the beginning than to add it in later. This is called mobile-first design. Rough out how the layout for desktop and mobile should work. Then worry about how the individual elements should look. Do lots of browser testing at this point to make sure the big-picture layout works on everything.

Most people build a desktop site and then set display:none; to hide things for mobile. This is less than ideal because the mobile browser still has to download those assets, and then set them to display:none. Instead, build your site mobile-first. Have the repsonsive media queries **add* things in as the site widens, instead of hide things.

### Tools:

* [Firefox responsive design view](https://developer.mozilla.org/en-US/docs/Tools/Responsive_Design_View)
* [Chrome device emulator](https://developer.chrome.com/devtools/docs/device-mode)
* [quirksmode.org](quirksmode.org)
* [list of device bugs](github.com/scottjehl/Device-Bugs/issues)


### Talks:

* [CSS in Your Pocket - Mobile CSS Tips from the Trenches](https://www.youtube.com/watch?v=vBHt61yDO9U) - has a lot of in depth discussion about particular gotchas not listed here


## Use a preprocessor

Most of the talks mentioned using a preprocessor of some kind. Preprocessors bring a lot of useful functionality like variables, functions, and automatic vendor prefixing on compile. Look into them and decide if it's right for you / your team.


### Blog posts:

* [Ten Reasons You Should Be Using a CSS Preprocessor](https://www.urbaninsight.com/2012/04/12/ten-reasons-you-should-be-using-css-preprocessor)
* [Pros and Cons](http://www.nosleepforsheep.com/development/using-a-css-preprocessor/)


### Tools:

* [Sass](https://github.com/sass/sass)
* [Less](https://github.com/less/less.js)
* [Stylus](https://github.com/LearnBoost/stylus)


## Think about a framework / UI toolkit

@Lucy - what is a framework vs. UI toolkit? Re: Bootstrap, a lot of people include ALL of Bootstrap just to get something like tooltips. Total waste.

Frameworks like Bootstrap can have a lot of problems[citation needed] so think it through before you use one. Some relevant questions:

* How will you use it (mixins or class names)?
* Will it help you in the long run or do you think you'll end up fighting it?
    *  If you need it for a quick prototype, but don't want it in your final version, does everyone invloved know it's going to go? How can you change your workflow to make it easier to remove it in the future?
* What are the pros and, more important, cons of this particular framework?
* Does it support all the browsers your site supports?
* Are you doing lots of custom CSS? Does the framework support this well?


### Tools:

* [Framework comparison](http://usablica.github.io/front-end-frameworks/compare.html)


### Talks:

* [What Is a CSS Framework Anyway?](https://vimeo.com/95734680)


## Have a CSS coding style guide

@Lucy look into style guide generators.

Be consistent:

* Are you going to write your own style guide or borrow someone else's? (Tip: check the references to see if anyone has a styleguide you already like)
* Pixels vs. ems vs. percentages - are the all allowed? Are there any suggestions about when to use what?
* Are you allowed to style IDs? elements?
* White space?
* Should all the CSS pass or go through a linter?
* If you're using a preprocessor, how do you break down your files for inclusion (by module, by page, something else)?
* How much should you comment?
* Do you care about ordering the properties in a given rule (alphabetical, browser prefixed first, other)?
* Include some "dos" and "don'ts".


### References:
* [List of style guides](http://css-tricks.com/css-style-guides/)
* [CSS Lint](http://csslint.net/) and a [post](https://2002-2012.mattwilcox.net/archive/entry/id/1054/) about why it might not be great


## Some Dos and Don'ts

These came from many talks, some of which contradicted each other. Hopefully these are an accurate overview of generally accepted best practices that came from the talks. Of course none of these "rules" should be followed exactly, there will always be exceptions. They're more like guidelines.

* Use separate stylesheets
    * Don't use inline or header style rules
* Have specific class names (`.header-title`)
    * Don't have long selectors (`.header .title h1`)
    * Don't use very specific selectors (`.head > #nav`)
* Take time to refactor periodically
    * Don't assume you need to start from scratch
    * Don't let bad code stay around - over time the quality will decrease
* Make your layout first
    * Don't add responsive after the fact
    * Test your layout in many browsers while you're creating it
* Decide which devices and browsers you're going to support and optimise for early on the life of the project
* Design with your content in mind
    * Don't design for the perfect case and ignore the rest
    * Don't change the design too close to release
* Care about performance
    * Use inline or header style rules for CSS you absolutely need, and defer the rest till the end of the page
    * Don't use the `*` selector
    * Don't use long selectors (again, like `.header .title h1`)
    * Don't use border-radius and box-shadow in combination (if you can help it)
    * Don't have unused CSS
* Think through specificity and plan out how your classes should interact
    * Don't chuck `!important` on your rules without justifying it
    * Decide on naming conventions to help clear up how the classes / modules interact

### Talks:

* [Slaying the Dragon: How to Refactor CSS for Maintainability](https://vimeo.com/100501790)
* [Architecting Scalable CSS](https://vimeo.com/70041549).
* [CSS in Your Pocket - Mobile CSS Tips from the Trenches](https://www.youtube.com/watch?v=vBHt61yDO9U)
* [CSS Is a Mess](https://vimeo.com/99877232)
