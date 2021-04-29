---
layout: post
title:  "HTML5 placeholder support for IE9"
---

Modern web applications use HTML5 placeholders to give a hint to the user about what to enter in a field.
<!--more-->

```html
<input type="text" placeholder="Enter your name" id="name"/>
```

The latest versions of all modern browsers [support](http://caniuse.com/#feat=input-placeholder) this attribute, but older versions of internet explorer (<IE10) don’t support it. This little snippet will add support for the placeholder attribute in browsers who lack this HTML5 feature.

[//]: # (Summary)

```javascript
<script src="//ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js"></script>
<script type="text/javascript">
$(function() {
    var input = document.createElement("input");
    if(('placeholder' in input)==false) {
      $('[placeholder]').focus(function() {
          var i = $(this);
          if(i.val() == i.attr('placeholder')) {
              i.val('').removeClass('placeholder');
              if(i.hasClass('password')) {
                  i.removeClass('password');
                  this.type='password';
              }            
          }
      }).blur(function() {
          var i = $(this);    
          if(i.val() == '' || i.val() == i.attr('placeholder')) {
              if(this.type=='password') {
                  i.addClass('password');
                  this.type='text';
              }
              i.addClass('placeholder').val(i.attr('placeholder'));
          }
      }).blur().parents('form').submit(function() {
          $(this).find('[placeholder]').each(function() {
              var i = $(this);
              if(i.val() == i.attr('placeholder'))
                  i.val('');
          })
      });
  }
});
</script>
```

In order to style the placeholder text, you can use the placeholder class added by this snippet.

```css
.placeholder {
  color: #bbb;
}
```
