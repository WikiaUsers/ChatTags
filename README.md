# ChatTags
ChatTags is a BBCode parser add-on for Wikia's Special:Chat extension. It allows users to add formatting to their in-chat messages. Such formatting includes the ability to add colors to their messages, change the font of their messages, alter the font size, alter the font weight, and the background of their messages.

This gives users the creative freedom to better express themselves while in chat.

Users are now able to bold important sections of their messages or even highlight them using colors and backgrounds instead of using all caps. They are also able to italicize quotes and what ever else they deem necessary.

Put together this script enhances chats of all purposes.

# Importing
This file is available thanks to [RawGit](https://rawgit.com) at the following URL:
`https://cdn.rawgit.com/WikiaUsers/ChatTags/v2.6.1/src/chattags.js`

Chat Tags can easily be installed by editing the following line into your wiki's **MediaWiki:Chat.js** page.
`mw.load.loader('https://cdn.rawgit.com/WikiaUsers/ChatTags/v2.6.1/src/chattags.js');`

Additionally, if you would like to enable the image or youtube tag you will need to add the following line **ABOVE** the script import.
`var chatags = { images: true, videos: true };`

# Usage
To use Chat Tags on your wiki after installation one must only wrap their text in the designated tags that you will find below. Chat Tags functions much like HTML in that it requires a beginning tag that shows where you wish to start formatting and ending tag to show when to stop formatting. If you do not include both of these tags then your text will be ignored by the script.

**Tags must end in the order that they start**

For example:

----

`[c="red"]This text will be red.[/c] -> <span style="color:red;">This text will be red.</span>`

`[c="#FF0000"]This text will also be red.[/c] -> <span style="color:#FF0000;">This text will also be red.</span>`

`[bg="red"]This text will have a red background.[/bg] -> <span style="background:red;">This text will have a red background.</span>`

`[b]This text will be bold.[/b] -> <span style="font-weight:bold;">This text will be bold.</span>`

`[c="red"][b]This text will be red and bold.[/b][/c] -> <span style="color:red;font-weight:bold;">This text will be red and bold.</span>`

----
## Tags

* Text color
 * `[c="<color code>"]<message>[/c]`
* Background color
 * `[bg="<color code>"]<message>[/bg]`
* Font
 * `[font="<font name>"]<message>[/font]`
* Code tag
 * `[code]<message>[/code]`
* Bold text
 * `[b]<message>[/b]`
* Italicize text
 * `[i]<message>[/i]`
* Big text
 * `[big]<message>[/big]`
* Small text
 * `[small]<message>[/small]`
* Subscript
 * `[sub]<message>[/sub]`
* Superscript
 * `[sup]<message>[/sup]`
* Strike through
 * `[s]<message>[/s]`
* Underline
 * `[u]<message>[/u]`
* Image
 * **Note**: Leave the http:// or https:// off of the link
 * `[img="<image>"]`
* Youtube
 * **Note**: the string that comes after `watch?v=` is the watch ID (`uQzGxQxn84Y` in the link `www.youtube.com/watch?v=uQzGxQxn84Y` for example)
 * `[yt="<video ID>"]`

== Developers ==
ChatTags is very much extensible and allows developers to easily add and remove tags as they see fit.

In order to add a tag you must first import ChatTags. After doing this you must add an entry to the tags object in order to do this you must pick a name that has not been previously chosen. All default tags are listed above. After selecting the name for your tag you must then decide whether it is stand-alone or a pair.

A stand alone tag only require and opening tag (see: img, yt).

A pair tag  requires both an opening and closing tag (see: c, u, bg)

After deciding the structure of your tag you must decide if you want it to accept a parameter or not. After deciding this you want to create your object entry. For example purposes we will be creating a user tag that turns their text color purple.

To add an entry we must do the following ***below*** our import.

```js
//... import ...
chatags.tags['user'] = function(s,t) {        // S is the user string, T is the tag
    if (t.charAt(0) === '/') {                // We have been given the ending tag
        s = s.replace('[/user]', '</span>');
    } else {                                  // We have been given the opening tag
        s = s.replace('[user]', '<span style="color:purple">');
    }
    return s;                                 // Return the user string for further parsing
};
```

Now, let's say that we want to let the user enter their own color, we'd need to do something like the following.

```js
//... import ...
chatags.tags['user'] = function(s,t) {
    if (t.charAt(0) === '/') {
        s = s.replace('[/user]', '</span>');
    } else {
        try {
            t = t.split(' ');
            s = s.replace('[user' + t[1] + ']', '<span style="color:' + mw.html.escape(t[1]) + ';">');
        } catch(e) { console.log(e) }
    }
    return s;
};
```

When dealing with user input you must **always** use `mw.html.escape()` to escape the input to prevent script injection.