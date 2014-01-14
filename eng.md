<section id="not-found-template" class="template">
## Not Found

Sorry, the page you are looking for does not exist in the Matrix.</section><
section id="validation-dialog-template" class="template
"><article class="dialog validation-dialog">

Close<header class="dialog-title">
## Oops!</header><section class="dialog-body"><section class="validation {{css
}}">

Close</section></section><footer class="dialog-buttons"></footer></article></
section
><section id="validation-template" class="template"><section class="validation
{{css
}}">Close</section></section><section id="file-upload-template" class="template
"><section class="upload-area">
Drop here to begin upload

Uploading file...</section></section><section id="expand-section-template"
class="template
"><section class="expand-section">

Expand{{legend}}</section></section><section id="table-pager-template" class="
template
"><section class="table-pager">{{legend}}</section></section><section id="
prompt-link-template" class="template
"><article class="dialog markdown-prompt-dialog">Close<header class="dialog-
title
">
## Insert Link</header><section class="dialog-body">

http://example.com/ "optional title"</section><footer class="dialog-buttons">

Cancel</footer></article></section><section id="prompt-image-template" class="
template
"><article class="dialog markdown-prompt-dialog">Close<header class="dialog-
title
">
## Insert Image</header><section class="dialog-body">

http://example.com/images/diagram.jpg "optional title"</section><footer class="
dialog-buttons
">

Cancel</footer></article></section><section id="blog-entries-template" class="
template
"><section class="blog-entries-wrapper"><section class="blog-entries"><article
class="blog-entry-section blog-entry
"><header class="blog-entry-title clearfix"><section class="social-shares">
[][1][][2]</section>


, posted {{timeAgo}}</header><section class="markdown blog-entry-body"><section
class="blog-entry-text">{{{html.text
}}}</section></section></article></section></section></section><section id="
empty-entry-template" class="template
"><article class="blog-entry-section blog-empty"><header class="blog-entry-
title
">

## No posts</header><section>

*{{emptyText}}**No posts found for {{title}}*</section></article></section><
section id="exhausted-entries-template" class="template
"><footer class="blog-entry-section blog-exhausted"><section>

*No more posts*</section></footer></section><section id="entry-siblings-
template" class="template
"><section class="blog-entry-siblings clearfix">

[{{previous.title}}][3]

[{{next.title}}][4]</section></section><section id="entry-pager-template" class
="template
"><footer class="blog-pager flip-wrapper"><article class="flip-card flip-x"><
section class="face front
"></section><section class="face back"></section></article></footer></section
><section id="more-entries-template" class="template"><article class="blog-
entry-section blog-entry
">

<header class="blog-entry-title clearfix"><section class="social-shares">[][1]
[][2]</section>


, posted {{timeAgo}}</header><section class="markdown blog-entry-body"><section
class="blog-entry-text">{{{html.text
}}}</section></section></article></section><section id="discussion-list-
template" class="template
"><section class="blog-entry-section blog-discussions"><article id="thread-{{
_id}}" class="blog-discussion
"></article></section></section><section id="user-profile-template" class="
template
"><section class="user-profile-actions">

[Edit][5][Logout][6]</section>
## {{profileTitle}}<article class="user-profile-card">

![][7]<section class="user-profile-info">
member for{{duration}}

website[{{website.titleText}}][8]</section></article><article class="user-
profile-bio
"><header>

### About</header></article></section><section id="authentication-required-
template" class="template
"><section class="authentication-required">

To {{action}}, please [log in][9] first

*No typing required!*</section></section><section id="user-password-reset-
template" class="template
">

## Type your new password<section class="password-reset-area">

You can change your password below</section></section>

 [1]: https://twitter.com/share
 [2]: https://getpocket.com/save
 [3]: http://blog.ponyfoo.com/2014/01/09/%7B%7Bprevious.url%7D%7D
 [4]: http://blog.ponyfoo.com/2014/01/09/%7B%7Bnext.url%7D%7D
 [5]: http://blog.ponyfoo.com/user/profile/%7B%7B_id%7D%7D/edit "Edit"
 [6]: http://blog.ponyfoo.com/user/logout
 [7]: img/
 [8]: http://blog.ponyfoo.com/2014/01/09/%7B%7Bwebsite.url%7D%7D
 [9]: http://blog.ponyfoo.com/user/login