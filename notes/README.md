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

## Table of Contents
  1. [Mental framework](#mental-framework)
  1. [Clear Class names](#clear-class-names)
  1. [Handy for search and replacee](#namespace-search)
    1. [By componentt](#namespace-component)
    1. [Keep your JS separate](#namespace-js)
    1. [During refactors](#namespace-refactor)
  1. [Remove unused CSS](#unused-css)
  1. [Use Mixins or Extends](#use-mixins)
  1. [Group code / modules logicially](#group-code)
  1. [Visual regression tests](#visual-regression-tests)
  1. [Responsive CSS](#responsive)
  1. [Use a preprocessor](#preprocessor)
  1. [Think about a framework / UI toolkit](#frameworks)
  1. [Have a style guide](#style-guide)
  1. [Some Dos and Don'ts](#dos-donts)

## <a name='mental-framework'>Mental framework</a>
A good framework for thinking about the topics presented in the talks was discussed in [Play Nice With CSS Tools and Methodologies](https://www.youtube.com/watch?v=-bZSTMLqf8Q&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA). I'd recommend watching that one early on just to have some clear categories for the other talks. He broke all discussions down into 4 categories:
1. Patterns
1. Implementation
1. Ideology
1. Methodology

In general I didn't take notes on implementation since it's likely to change, it's (fairly) easy to Google, and it's only applicable in certain cases.


## <a name='clear-class-names'>Clear Class names</a>
This topic seemed to come up the most. There were differing opinions about how to define "clear," some of which are mentioned below. No matter how you decide to define "clear" it should be consistent and the entire team should subscribe to the same method.

"Names shoud clarify intent."


Don't tie the name to how the element looks (`.red-alert` vs. `.alert`) - what happens if the color changes?

Don't use too many class names (`.btn .big-btn .click-me` vs `.action-btn`). (See: [use mixins]((#use-mixins)) Or do the reverse...
###<a name='ideologies'> Ideologies</a>
* Some people think that class names should deal with the content of an element (e.g. `.book-title`). This can have problems when the code is written literally (`.new-roman-title`) and then something changes (`.new-roman-title` now needs to be in helvetica).
* Others think the class names should only handle what the content looks like (e.g. `.large-text`). (Hint: Twitter-Bootstrap is, by necessity, this option since it doesn't know what your content is.) This can lead to lots of classes (`.small.box.border.rounded-coners`).

###<a name='namespace-class-names'> Name space (prefix/postfix) your class names</a>
#### <a name='namespace-search'>Handy for search and replace</a>
If you ever have to do a global find and replace, it'll be much easier if your class names are quite unique (`.title` vs. `.book-title`). Also, CSS is faster for individual class names than for many nested ones (`.book-title` vs `.book .title`).

#### <a name='namespace-component'>By component</a>
It's nice to name space your class names to the component they're for (`.heading` vs. `.profile-heading`). It will help keep similar items together, as well as letting you know where the class is used. This can also be useful for new features that may not go live - you'll be able to tell which CSS is relevant and delete it if needed.

#### <a name='namespace-js'>Keep your JS separate</a>
It's also a good idea to prefix the class names you use for JS. (Think `.modal-btn` vs. `.modal-btn .js-modal`.) That way the CSS should never need to change the class names that JS uses.

Or you could use the data attributes. As it [turns out](http://stackoverflow.com/questions/9181526/jquery-performance-select-by-data-attr-or-by-class) it's about the same speed.

#### <a name='namespace-refactor'>During refactors</a>
Namespace your old selectors with `-old`. (You could namespace your new selectors with `-new`, but then you'll need to update them all when you remove the old styles.) This has advantages:
1. you know you're not going to conflict with exiting styles,
1. you can do a global find and replace to remove the old styles, and
1. it's clear to people not participating in your refactor which rules will stick around.

### Tools:
* [Object Oriented CSS](https://github.com/stubbornella/oocss/wiki)
* [BEM](http://bem.info/method/) and [thinking about BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

### Talks:
* [When Bootstrap Attacks](https://www.youtube.com/watch?v=xbpnqbM6cRk&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA)
* [Slaying the Dragon: How to Refactor CSS for Maintainability](https://vimeo.com/100501790)
* [Play Nice With CSS Tools and Methodologies](https://www.youtube.com/watch?v=-bZSTMLqf8Q&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA)
* [CSS Is a Mess](https://vimeo.com/99877232)
* [Architecting Scalable CSS](https://vimeo.com/70041549)


## <a name='unused-css'>Remove unused CSS</a>

The CSS file is larger if you have CSS that you're not using, therefore your site loads more slowly. It's best practice to remove rules that the site doesn't need.

However, it can be pretty difficult to know which CSS rules are absolutely not used. There are a few tools for this, listed below. You can also try adding a postfix for any existing class names you don't think are being used (something like `-to-delete`). Then go over your site, see if anything is broken, and find which of the class names have the postfix and are on that page. Of course, this is easier if you have visual regression tests to go over your site to find broken elements.

It's a good idea to check your site periodically so you only have to remove a few rules at a time rather than a bunch all in one go.

Often these rules are from bootstrap or the like, so they can be hard to remove on your own. Think about using mixins on your own classes rather than including everything from bootstrap.

### Tools:
* [uncss](https://github.com/giakki/uncss)
* You can use a grunt task to run over your site and only write the CSS rules you actually use.

### Talks:
* [Automating the Removal of Unused CSS](https://www.youtube.com/watch?v=833xr1MyE30&list=PLUS3uVC08ZaqVEGFkl_dS_3FUzILkOIzA)

## <a name='use-mixins'>Use Mixins or Extends</a>
If you're using a preprocessor you can use mixins or extends to:
* minimize duplication of code,
* only write the rules for selectors you're using (rather than bulk importing something like Bootsrap),
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

## <a name='group-code'>Group code / modules logically</a>
A future developer is going to make changes. How can you make their life as easy as possible? (Reminder: This future developer is likely to be you.)

Think of your site in pieces (modules); how you can reuse these modules?
* Group code for the same module together
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


## <a name='visual-regression-tests'>Visual regression tests</a>
Use visual regression testing to find any changes to the display of the site after CSS changes. Visual regression tests are generally quite brittle (you might not care about that 1px shift, but it will register as a test fail), but might make you more secure when you refactor any CSS.

This certainly isn't industry standard yet, or even an agreed "good practice" but it might make your life easier, so it's probably worth looking into.

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


## <a name='responsive'>Responsive CSS</a>
It's must easier to make the CSS responsive from the beginning than to add it in later. Rough out how the layout for desktop and mobile should work first before getting concerned about how the individual elements should look. Do lots of browser testing at this point to make sure the big-picture layout works on everything.

Note: Android 2.3 is really behind, if you're going to support it, make sure you're testing with it from the beginning

### Tools:
* [Firefox responsive design view](https://developer.mozilla.org/en-US/docs/Tools/Responsive_Design_View)
* [Chrome device emulator](https://developer.chrome.com/devtools/docs/device-mode)
* [quirksmode.org](quirksmode.org)
* [list of device bugs](github.com/scottjehl/Device-Bugs/issues)

### Talks:
* [CSS in Your Pocket - Mobile CSS Tips from the Trenches](https://www.youtube.com/watch?v=vBHt61yDO9U) - has a lot of in depth discussion about particular gotchas not listed here

## <a name='preprocessor'>Use a preprocessor</a>
Most of the talks mentioned using a preprocessor of some kind. Look into them and decide if it's right for you/your team.

### Blog posts:
* [Ten Reasons You Should Be Using a CSS Preprocessor](https://www.urbaninsight.com/2012/04/12/ten-reasons-you-should-be-using-css-preprocessor)
* [Pros and Cons](http://www.nosleepforsheep.com/development/using-a-css-preprocessor/)

### Tools:
* [Sass](https://github.com/sass/sass)
* [Less](https://github.com/less/less.js)
* [Stylus](https://github.com/LearnBoost/stylus)

## <a name='frameworks'>Think about a framework / UI toolkit</a>
Frameworks can have a lot of problems so think it through before you use one. Some relevant questions:
* How will you use it (mixins or class names)?
* Will it help you in the long run or do you think you'll end up fighting it?
    *  If you need it for a quick prototype, but don't want it in your final version, does everyone invloved know it's going to go? How can you change your workflow to make it easier to remove it in the future?
* What are the pros and, probably more important, cons of this particular framework?
* Does it support all the browsers your site is supposed to support?
* Are you doing lots of custom CSS? Does the framework support this well?

### Tools:
* [Framework comparison](http://usablica.github.io/front-end-frameworks/compare.html)

### Talks:
* [What Is a CSS Framework Anyway?](https://vimeo.com/95734680)

## <a name='style-guide'>Have a style guide</a>
Be consistent:
* Are you going to write your own style guide or borrow someone else's? (Tip: check the references to see if anyone has a styleguide you already like)
* Pixels vs. ems vs. percentages - are the all allowed? Are there any suggestions about when to use what?
* Are you allowed to style IDs? elements?
* White space?
* Should all the CSS pass or be run through a linter?
* If you're using a preprocessor, how do you break down your files for inclusion (by module, by page, something else)?
* How much should you comment?
* Do you care about ordering the properties in a given rule (alphabetical, browser prefixed first, other)?
* Include some "dos" and "don'ts".

### References:
* [List of style guides](http://css-tricks.com/css-style-guides/)
* [CSS Lint](http://csslint.net/) and a [post](https://2002-2012.mattwilcox.net/archive/entry/id/1054/) about why it might not be great


## <a name='dos-donts'>Some Dos and Don'ts</a>
These came from many talks, some of which contradicted each other. Hopefully these are an accurate overview of generally accepted best practices that came from the talks. Of course none of these "rules" should be followed exactly, there will always be exceptions. They're more like guidelines.

* Use separate style sheets
    * Don't use inline or header style rules
* Have specific class names (`.header-title`)
    * Don't have long selectors (`.header .title h1`)
    * Don't use very specific selectors (`.head > #nav`)
* Take time to refactor periodically
    * Don't assume you need to start from scratch
    * Don't let bad code stay around - over time the overall quality will decrease
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
    * Don't use border-radius, box-shadow, or transform (if you can help it)
    * Don't have unused CSS
* Think through specificity and plan out how your classes should interact
    * Don't chuck `!important` on your rules without really justifying it
    * Decide on naming conventions to help clear up how the classes / modules interact

### Talks:
* [Slaying the Dragon: How to Refactor CSS for Maintainability](https://vimeo.com/100501790)
* [Architecting Scalable CSS](https://vimeo.com/70041549).
* [CSS in Your Pocket - Mobile CSS Tips from the Trenches](https://www.youtube.com/watch?v=vBHt61yDO9U)
* [CSS Is a Mess](https://vimeo.com/99877232)