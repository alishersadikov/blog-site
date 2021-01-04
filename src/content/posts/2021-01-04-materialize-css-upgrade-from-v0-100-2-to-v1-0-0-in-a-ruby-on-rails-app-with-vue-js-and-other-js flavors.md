---
template: blog-post
title: Materialize CSS upgrade from v0.100.2 to v1.0.0 in a Ruby on Rails app
  with Vue.js and other JS flavors
slug: materialize-upgrade
date: 2019-05-26 18:17
description: materialize upgrade
featuredImage: /assets/time-to-upgrade.jpg
---
Although I do not deal with UX/UI design too much, I think material design looks great. As full stack developers, we are usually not so excited about writing our own CSS, so we turn to CSS frameworks. Every framework has its ups and downs, but the ease of upgrading and backward compatibility are things all of us appreciate. And sometimes we have to upgrade regardless.

### Why upgrade now?

Although [Materialize CSS](https://materializecss.com) v1.0.0 was released a couple years ago now, my current project was still using v0.100.2 and we were pretty comfortable with the occasional minor patches. Then with Chrome 73 came more painful stuff — mostly broken datepickers and dropdowns for us (globally in the app). Needless to say, the old version was not maintained any more. I think [this chromium issue](https://bugs.chromium.org/p/chromium/issues/detail?id=941910) captures the Chrome 73 related problem(s) really well at a low level.

I was able to mitigate (not eliminate — of course there are trade-offs!) those pains with a patch to v0.100.2 and materialize maintainers were open to merging [my PR](https://github.com/Dogfalo/materialize/pull/6339). That was a temporary fix though. For a more permanent fix, I got buy-in from my team to do the upgrade to v1.0.0 ASAP.

This article is about how that went…

![](https://cdn-images-1.medium.com/max/800/0*-51bXF7iQtLIhAVl.jpg)

### Planning the work

I tried to be smart about planning it, but these attempts to figure out the path of minimum resistance were not so helpful:

1. Analyzing the official [Upgrade to v1.0.0 from v0.100.2](https://github.com/Dogfalo/materialize/blob/v1-dev/v1-upgrade-guide.md) guide which is more like a change log than specific step-by-step instructions
2. Asking around about people’s experiences to get an idea of the major pain points

In addition to the upgrade guide, [this medium article](https://medium.com/@materializecss/materialize-to-1-0-and-beyond-e0233b8ac15) by materialize folks is really useful in giving you the general idea and rationale behind the sweeping changes.

### My project setup

Obviously, depending on the tech stack and other dependencies, the upgrade will take a different shape and route for every code base. I think that is exactly the reason why materialize folks could not lay out the specific steps — it is just impossible and counter intuitive.

Some details of my code base that went through the upgrade:

1. Rails 5.2 with webpack: used as full-stack with HAML views as well as structured as just API in a detached client-server setup
2. Many flavors of JavaScript: vanilla, jQuery, CoffeeScript, and Vue.js
3. Heavy internationalization, including RTL 😩.

Keep in mind it is the NPM package we are upgrading, so this could be useful for any project using the package.

### 1. Install the package

Depending on your JavaScript package management tool (see the [readme on npmjs.com](https://www.npmjs.com/package/materialize-css) for options), run the relevant command to install the materialize-css package, in our case we use yarn:

```
yarn add materialize-css@latest
```

And yes, this will replace the old version if you already have it.

### 2. Materialize initialization

We have *app/javascript/materialize* folder where we initialize our materialize scss and JavaScript — you probably have a similar structure. In the old version, materialize was initialized in our *app/javascript/materialize/index.js* like so:

```
window.materializeInitialize = new Event('materialize.initialize');
```

And then dispatching the event wherever materialize was needed:

```
window.dispatchEvent(window.materializeInitialize);
```

While global initialization of all components is still an option with the new version:

```
M.AutoInit();
```

I personally do not like doing that and it also does not seem to be the style of the new version. And in an app like mine, where there is so many flavors of JavaScript, global init did cause some double events and other bad things to fire. So instead, I only initialized the components/plugins I was using on an as-needed basis and where they are needed:

```
document.addEventListener('DOMContentLoaded', function() {
  M.FormSelect.init(document.querySelectorAll('select'),{})
  M.Dropdown.init(document.querySelectorAll('.dropdown-trigger'), {
   coverTrigger: false,
   constrainWidth: false
  });
  // and so on ...
});
```

I will use the term *plugins* when referring to materialize components from here on.

On the downside of this incremental approach, if you have too many pages, it will be hard to track down all the plugins you were using before, but they might still be broken with the global init option — due to syntax/internal changes.

[Materialize documentation](https://materializecss.com/) is pretty good and it usually lists two options to initialize the plugins: with vanilla JS or jQuery. I chose the former because jQuery is no longer a dependency and I try to avoid jQuery code in my Vue.js components (as much as I can). So the initialization is the time for you to choose either format and it makes sense to stick to that format moving forward.

If you have a custom Vue.js component that relies on materialize, you are recommended to initialize the relevant plugin (in this case — select) in the mounted life cycle hook:

```
export default {
  mounted() {
    M.FormSelect.init(select);
 },
...
};
```

### 3. Colors

![](https://cdn-images-1.medium.com/max/800/0*yRyQKOYa4zTxPdDo.jpg)

If pink is not your primary color and if you were using the color component before, you will probably see a lot of pink. Maybe time to change the color scheme? If not, find the import(s) similar to this:

```
@import "~materialize-css/sass/components/color";
```

and change it to:

```
@import "~materialize-css/sass/components/color-variables";
```

By this time I hope webpack compiles, but it is normal to see a bunch of other errors — plugin specific ones.

### 4. Fonts

The new version reduces the dependencies and along those lines, the default Roboto font does not come with the package any more. Maybe it is time to change your fonts? I did not think so and still wanted to use all the font variables I had in my css/scss files.

The huge mistake I made in the beginning was thinking in the pre-Rails 5 mindset of the asset pipeline. So do not put your fonts in *app/assets* directory. In our case we specify what packs to build in *webpacker.yml*:

```
default: &default
 source_path: app/javascript
 source_entry_path: packs
 public_output_path: packs
```

And so I downloaded all Roboto fonts the old materialize had from [Google Fonts](https://fonts.google.com/specimen/Roboto?selection.family=Roboto) and put them into *app/javascript/fonts*, which are these:

```
Roboto-Thin.woff2, Roboto-Thin.woff       for font-weight 100;
Roboto-Light.woff2, Roboto-Light.woff     for font-weight 300;
Roboto-Regular.woff2, Roboto-Regular.woff for font-weight 400;
Roboto-Medium.woff2, Roboto-Medium.woff   for font-weight 500;
Roboto-Bold.woff2, Roboto-Bold.woff       for font-weight 700;
```

Then I defined font-faces in *app/javascript/materialize/styles.scss* for every single group of fonts above, with the font-weight:

```
@font-face {
 font-family: "Roboto";
 src: url("../fonts/Roboto-Thin.woff2") format("woff2"),
      url("../fonts/Roboto-Thin.woff") format("woff");
 font-weight: 100;
}
```

It would be more accurate to say that I changed the definition of the existing font-faces, because those font-faces were pointing to the library contents which do not exist any longer.

Although you are now pretty much all set fonts-wise, there is one variable I had to dig out of the source code that will still override your fonts and so you will probably not see the effects of the above — it is the *$font-stack* variable. Somewhere in your scss for materialize put the following with your choice of the secondary font:

```
$font-stack: Roboto, sans-serif;
```

And now you are ready to restart the *webpack-dev-server* and behold your old Roboto fonts… (hopefully).

### 5. Component/plugin specific changes

This part was more fun. I am not going to list all the plugins and how to fix/upgrade them, but I will just list the ones (mostly examples) I had to fix in my app and only interesting ones among those. So this is going to be a smaller sample. Wherever possible, I am showing HAML code here, since it is harder to find examples of it. The more universal HTML version is beautifully documented my materialize.

#### Dropdowns:

An example of a working dropdown (HAML):

```
= link_to '#', 'data-target': 'certain-dropdown', class: 'dropdown-trigger' do
    %i.small.material-icons.icon settings

  %ul.dropdown-content#certain-dropdown
    %li.divider
      %li= link_to t('dropdowns.first'), first_dropdown_item_path
    %li.divider
      %li= link_to t('dropdowns.second'), second_dropdown_item_path
```

And initialize with your desired options:

```
M.Dropdown.init(document.querySelectorAll('.dropdown-trigger'), {
  coverTrigger: false,
  constrainWidth: false
});
```

#### Checkboxes:

(HAML)

```
%input{type: 'checkbox', checked: false ...}
%label= t('.check_box_text')
```

to:

```
%label
  %input{type: 'checkbox', checked: false ...}
  %span= t('.check_box_text')
```

Notice the nesting.

#### Radio buttons:

Same kind of nesting as the checkbox.

#### Toast messages:

(JavaScript)

```
Materialize.toast(I18n.t('key.another_key.value'), 4000);
```

To:

```
M.toast({html: I18n.t('key.another_key.value')});
```

Notice the nesting under the html key and 4000 is the default timeout now.

#### Selects:

```
$('select').material_select();
```

to something like:

```
M.FormSelect.init(document.querySelectorAll('select'));
```

#### Tooltips:

```
M.Tooltip.init(document.querySelectorAll('.tooltipped'));
```

The other plugins I had to upgrade were pretty easy and not worth mentioning. Again documentation does a pretty good job there, except for what comes next.

### 5. Datepicker

Because of the effort that went into this, definitely worth a separate section.

To quote [the upgrade guide](https://github.com/Dogfalo/materialize/blob/v1-dev/v1-upgrade-guide.md#datepicker):

* Complete rewrite of Datepicker, please see new documentation
* Rename plugin call from `.pickadate()` to `.datepicker()`
* Datepicker options `clear`, `close` moved to `i18n.clear`, and `i18n.done` respectively.

Here is my old datepicker Vue component (I know, some jQuery here 😌):

```
import $ from 'jquery';
import 'materialize-css';
import 'materialize-css/js/date_picker/picker';
import 'materialize-css/js/date_picker/picker.date';
export const DatePicker = {
  inserted(el) {
    $(el).pickadate({
      hiddenName: true,
      formatSubmit: 'yyyy/mm/dd',
      labelMonthNext: I18n.t('next_month', {scope: 'date_picker'}),
      labelMonthPrev: I18n.t('pre_month', {scope: 'date_picker'}),
      labelMonthSelect: I18n.t('select_month', {scope: 'date_picker'}),
      labelYearSelect: I18n.t('select_year', {scope: 'date_picker'}),
      weekdaysShort: _.compact(I18n.t('date.abbr_day_names')),
      monthsFull: _.compact(I18n.t('date.month_names')),
      monthsShort: _.compact(I18n.t('date.abbr_month_names')),
      weekdaysFull: _.compact(I18n.t('date.day_names')),
      weekdaysLetter: _.compact(I18n.t('date.abbr_day_names')),
      today: I18n.t('today', {scope: 'date_picker'}),
      clear: I18n.t('clear', {scope: 'date_picker'}),
      close: I18n.t('close', {scope: 'date_picker'}),
      onSet: function(date) {
        el.dispatchEvent(new CustomEvent('input', {'detail': { "date": $(el).siblings('[name='+el.id+']')[0].value }}))
      }
    })
  }
};
```

Here is the new one:

```
import $ from 'jquery';
import 'materialize-css';
export const DatePicker = {
  inserted(el) {
    $(el).datepicker({
      showClearBtn: true,
      format: 'mmm dd yyyy',
      i18n: {
        months: _.compact(I18n.t('date.month_names')),
        monthsShort: _.compact(I18n.t('date.abbr_month_names')),
        weekdays: _.compact(I18n.t('date.day_names')),
        weekdaysShort: _.compact(I18n.t('date.abbr_day_names')),
        weekdaysAbbrev: _.compact(I18n.t('date.abbr_day_names')),
        today: I18n.t('today', {scope: 'date_picker'}),
        cancel: I18n.t('cancel', {scope: 'date_picker'}),
        clear: I18n.t('clear', {scope: 'date_picker'}),
        done: I18n.t('done', {scope: 'date_picker'}),
        nextMonth: I18n.t('next_month', {scope: 'date_picker'}),
        previousMonth: I18n.t('pre_month', {scope: 'date_picker'}),
      },
      onClose: function() {
        el.dispatchEvent(new CustomEvent('input', {'detail': { "date": el.value }}));
      }
    })
  }
};
```

Notice these high-level changes:

1. The pickadate() to datepicker() change. The old datepicker relied on the [pickadate library](https://amsul.ca/pickadate.js/) and now materialize came up with its own thing. Needless to say a lot changed.
2. Nesting of the labels under I18n key — the documentation seems to lack this. I had to play around to finally arrive at this. Also notice a lot of the option/label keys have changed. To shed some light on those I18n values, some of them come from my own .yml files and some from Rails (magic!).
3. The onSet() life cycle hook does not exist any more and there is no equivalent. I went with onClose() because it seemed closer, but it is very different. It took me a while to grasp (with help of an in-house Vue.js expert) why so many things that relied on it were broken: before, an event was dispatched when a date was clicked in the picker. Now, it is dispatched when the picker is closed, which happens afterwards and it turned out to be too late in most cases. So you might be facing some adjustments there.

![](https://cdn-images-1.medium.com/max/800/0*OtXkvv2WquOV2qT_.jpg)

While this is our most primitive datepicker, we have another Vue-Form-Generator datepicker that builds on top of this one. Think of it as the Hulk of datepickers, but I will not comment on it at this time. However, one thing that is worth noting is that, the onClose() hook lost the localization of the date value and so we had to adjust it like so:

```
onClose(){
  el.dispatchEvent(new CustomEvent('input', {'detail': { "date": this.date, "date_string": el.value }}));
}
```

And of course you have to listen to this event now which I am not going to add the code for.

RTL and datepicker combination is a huge topic on its own, which I will not go into (in the readers’ best interests 😏). But I did have to put in some styles in order to make my RTL datepicker not so horrible:

```
.datepicker-table {
  th {
    font-size: 75%;
  }

  button.datepicker-day-button {
    font-size: 75%;
  }
}

.datepicker-date-display span.date-text {
  font-size: 2.5rem;
  padding-top: 1rem;
}
```

This upgrade was somewhat painful but not too painful. There is probably not another identical code base to ours and that difference will likely dictate the relevance. Nonetheless, I hope I covered some bits and pieces in this article that will make other people’s upgrade easier. Happy upgrading!