Use `http://www.reddit.com/api/comment` to post a comment.

## POST Data

| **field** | **sample value** | **explanation** |
|:----------|:-----------------|:----------------|
| `thing_id` or `parent`      | `t3_hzko5`, `t1_c1znqz2`       | The FULLNAME (see **glossary** on the [[API]] page) of the thing or comment you are commenting on. |
| `text`     | `Hey!\n\n**[search!](http://www.google.com/)**`              | The markdown content of the comment you are posting.  |
| `uh`      | `f0f0f0f0`...     | The currently logged in user's modhash (see **glossary** on the [[API]] page). |

## Example Response

If the `reddit_session` cookie **and** `uh` POST Data are not present in the request, the API will return something like the following:

```javascript
{
    "jquery": [
        [0, 1, "attr", "refresh"],
        [1, 2, "call", []],
        [0, 3, "attr", "find"],
        [3, 4, "call", [".error.USER_REQUIRED"]],
        [4, 5, "attr", "show"],
        [5, 6, "call", []],
        [6, 7, "attr", "text"],
        [7, 8, "call", ["please login to do that"]],
        [8, 9, "attr", "end"],
        [9, 10, "call", []]
    ]
}
```

Otherwise, the API will return JSON data with a series of serialized jquery commands to append the new comment to the page. The following is an example:

```javascript
{
   "jquery":[
      [0, 1, "call", ["#form-t3_iqhw5ili"]],
      [1, 2, "attr", "find"],
      [2, 3, "call", [".status"]],
      [3, 4, "attr", "hide"],
      [4, 5, "call", []],
      [5, 6, "attr", "html"],
      [6, 7, "call", [""]],
      [7, 8, "attr", "end"],
      [8, 9, "call", []],
      [1, 10, "attr", "find"],
      [10, 11, "call", ["textarea"]],
      [11, 12, "attr", "attr"],
      [12, 13, "call", ["rows", 3]],
      [13, 14, "attr", "html"],
      [14, 15, "call", [""]],
      [15, 16, "attr", "val"],
      [16, 17, "call", [""]],
      [0, 18, "attr", "insert_things"],
      [18, 19, "call", [
            [{
                  "data":{
                     "content":"<div class=\\"thing id-t1_c25u9i3 even odd comment \\" onclick=\\"click_thing(this)\\"><p class=\\"parent\\"><a name=\\"c25u9i3\\" ></a></p><div class=\\"midcol likes\\" ><div class=\\"arrow upmod\\" onclick=\\"$(this).vote('f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0', null, event)\\" ></div><div class=\\"arrow down\\" onclick=\\"$(this).vote('f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0f0', null, event)\\" ></div></div><div class=\\"entry likes\\"><div class=\\"collapsed\\" style='display:none'><a href=\\"#\\" class=\\"expand\\" onclick=\\"return showcomment(this)\\">[+]</a><a href=\\"http://www.reddit.com/user/KerrickLong\\" class=\\"author gray submitter id-t2_4appr\\" >KerrickLong</a><span class=\\"userattrs\\">&#32;[<a class=\\"submitter\\" title=\\"submitter\\" href=\\"/r/MostlyHarmless/comments/iqhw5/changelog_mostly_harmless_v041_released/\\">S</a>]</span>&#32;<span class=\\"score dislikes\\">-1 points</span><span class=\\"score unvoted\\">0 points</span><span class=\\"score likes\\">1 point</span>&#32;<time title=\\"Fri Jul 15 16:57:16 2011 GMT\\" datetime=\\"2011-07-15T16:57:16.209277+00:00\\">91 milliseconds</time>&#32;ago &nbsp;<a href=\\"#\\" class=\\"expand\\" onclick=\\"return showcomment(this)\\">(0 children)</a></div><div class=\\"noncollapsed\\" ><p class=\\"tagline\\"><a href=\\"#\\" class=\\"expand\\" onclick=\\"return hidecomment(this)\\">[&ndash;]</a><a href=\\"http://www.reddit.com/user/KerrickLong\\" class=\\"author submitter id-t2_4appr\\" >KerrickLong</a><span class=\\"userattrs\\">&#32;[<a class=\\"submitter\\" title=\\"submitter\\" href=\\"/r/MostlyHarmless/comments/iqhw5/changelog_mostly_harmless_v041_released/\\">S</a>]</span>&#32;<span class=\\"score dislikes\\">-1 points</span><span class=\\"score unvoted\\">0 points</span><span class=\\"score likes\\">1 point</span>&#32;<time title=\\"Fri Jul 15 16:57:16 2011 GMT\\" datetime=\\"2011-07-15T16:57:16.209277+00:00\\">91 milliseconds</time>&#32;ago</p><form action=\\"#\\" class=\\"usertext\\" onsubmit=\\"return post_form(this, 'editusertext')\\" id=\\"form-t1_c25u9i38va\\"><input type=\\"hidden\\" name=\\"thing_id\\" value=\\"t1_c25u9i3\\"/><div class=\\"usertext-body\\"><div class=\\"md\\"><p>Hey!</p><p><strong><a href=\\"http://www.google.com/\\">search!</a></strong></p></div>\\n</div><div class=\\"usertext-edit\\" style=\\"display: none\\"><div><textarea rows=\\"1\\" cols=\\"1\\" name=\\"text\\" >test&#32;comment</textarea></div><div class=\\"bottom-area\\"><span class=\\"help-toggle toggle\\" style=\\"display: none\\"><a class=\\"option active \\" href=\\"#\\" tabindex=\\"100\\" onclick=\\"return toggle(this, helpon, helpoff)\\" >formatting help</a><a class=\\"option \\" href=\\"#\\">hide help</a></span><span class=\\"error TOO_LONG field-text\\" style=\\"display:none\\"></span><span class=\\"error RATELIMIT field-ratelimit\\" style=\\"display:none\\"></span><span class=\\"error NO_TEXT field-text\\" style=\\"display:none\\"></span><span class=\\"error TOO_OLD field-parent\\" style=\\"display:none\\"></span><span class=\\"error DELETED_COMMENT field-parent\\" style=\\"display:none\\"></span><span class=\\"error DELETED_LINK field-parent\\" style=\\"display:none\\"></span><span class=\\"error USER_BLOCKED field-parent\\" style=\\"display:none\\"></span><div class=\\"usertext-buttons\\"><button type=\\"submit\\" onclick=\\"\\" class=\\"save\\" style='display:none'>save</button><button type=\\"button\\" onclick=\\"cancel_usertext(this)\\" class=\\"cancel\\" style='display:none'>cancel</button><span class=\\"status\\"></span></div></div><table class=\\"markhelp md\\" style=\\"display: none\\"><tr style=\\"background-color: #ffff99; text-align: center\\"><td><em>you type:</em></td><td><em>you see:</em></td></tr><tr><td>*italics*</td><td><em>italics</em></td></tr><tr><td>**bold**</td><td><b>bold</b></td></tr><tr><td>[reddit!](http://reddit.com)</td><td><a href=\\"http://reddit.com\\">reddit!</a></td></tr><tr><td>* item 1<br/>* item 2<br/>* item 3</td><td><ul><li>item 1</li><li>item 2</li><li>item 3</li></ul></td></tr><tr><td>&gt; quoted text</td><td><blockquote>quoted text</blockquote></td></tr><tr><td>Lines starting with four spaces<br/>are treated like code:<br/><br/><span class=\\"spaces\\">&nbsp;&nbsp;&nbsp;&nbsp;</span>if 1 * 2 &lt 3:<br/><span class=\\"spaces\\">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span>print \\"hello, world!\\"<br/></td><td>Lines starting with four spaces<br/>are treated like code:<br/><pre>if 1 * 2 &lt 3:<br/>&nbsp;&nbsp;&nbsp;&nbsp;print \\"hello, world!\\"</pre></td></tr><tr><td>~~strikethrough~~</td><td><strike>strikethrough</strike></td></tr><tr><td>super^script</td><td>super<sup>script</sup></td></tr></table></div></form><ul class=\\"flat-list buttons\\"><li class=\\"first\\"><a href=\\"http://www.reddit.com/r/MostlyHarmless/comments/iqhw5/changelog_mostly_harmless_v041_released/c25u9i3\\" class=\\"bylink\\" rel=\\"nofollow\\" >permalink</a></li><li><a class=\\"edit-usertext\\" href=\\"javascript:void(0)\\" onclick=\\"return edit_usertext(this)\\">edit</a></li><li><form class=\\"toggle del-button\\" action=\\"#\\" method=\\"get\\"><input type=\\"hidden\\" name=\\"executed\\" value=\\"deleted\\"/><span class=\\"option active\\"><a href=\\"#\\" onclick=\\"return toggle(this)\\">delete</a></span><span class=\\"option error\\">are you sure? &#32;<a href=\\"javascript:void(0)\\" class=\\"yes\\" onclick='change_state(this, \\"del\\", hide_thing)'>yes</a>&#32;/&#32;<a href=\\"javascript:void(0)\\" class=\\"no\\" onclick=\\"return toggle(this)\\">no</a></span></form></li><li><form action=\\"/post/remove\\" method=\\"post\\" class=\\"state-button remove-button\\"><input type=\\"hidden\\" name=\\"executed\\" value=\\"removed\\" /><span><a href=\\"javascript:void(0)\\" onclick=\\"return change_state(this, 'remove');\\">remove</a></span></form></li><li class=\\"toggle\\"><form method=\\"post\\" action=\\"/post/distinguish\\"><input type=\\"hidden\\" value=\\"distinguishing...\\" name=\\"executed\\"/><a href=\\"javascript:void(0)\\" onclick=\\"return toggle_distinguish_span(this)\\">distinguish</a><span class=\\"option error\\">distinguish this? &#32;<a href=\\"javascript:void(0)\\" onclick=\\"return set_distinguish(this, 'yes')\\">yes</a>&#32;/&#32;<a href=\\"javascript:void(0)\\" onclick=\\"return set_distinguish(this, 'no')\\">no</a>&#32; /&#32;<a class=\\"nonbutton\\" href=\\"/help/moderation#Distinguishing\\">help</a>&#32;</span></form></li><li><a class=\\"\\" href=\\"javascript:void(0)\\" onclick=\\"return reply(this)\\">reply</a></li></ul></div></div><div class=\\"child\\" ></div><div class=\\"clearleft\\"><!--IE6sux--></div></div><div class=\\"clearleft\\"><!--IE6sux--></div>",
                     "contentHTML":"<div class=\\"md\\"><p>Hey!</p><p><strong><a href=\\"http://www.google.com/\\">search!</a></strong></p></div>",
                     "contentText":"Hey!\n\n**[search!](http://www.google.com/)**",
                     "id":"t1_c25u9i3",
                     "link":"t3_iqhw5",
                     "parent":"t3_iqhw5",
                     "replies":""
                  },
                  "kind":"t1"
               }],
            false
         ]
      ],
      [0, 20, "call", ["#noresults"]],
      [20, 21, "attr", "hide"],
      [21, 22, "call",[]]
   ]
}
```

## API Reference: comment

Note: This reference ignores the serialized data and assumes you begin at `jquery[18][3][0][0]`.

- ### data *(object)*

    Holds relevant data about the comment.

    - ### content *(string)*

        Holds the HTML markup to insert into the reddit.com website.

    - ### contentHTML *(string)*

        Holds the HTML markup of the comment.

    - ### contentText *(string)*

        Holds the original source (in markdown) of the comment.

    - ### id *(string)*

        The FULLNAME (see **glossary** on the [[API]] page) of the comment.

    - ### link *(string)*

        The FULLNAME (see **glossary** on the [[API]] page) of the link the comment belongs to.

    - ### parent *(string)*

        If the comment was a reply, this is the FULLNAME (see **glossary** on the [[API]] page) of the comment that was replied to.

        If the comment was a root comment on a post, this is the FULLNAME (see **glossary** on the [[API]] page) of the link that was commented on.

    - ### replies *(string)*

        This will always be an empty string, because a brand new comment cannot have any replies.

- ### kind *(string)*

    The type of thing (see **glossary** on the [[API]] page) this is, which translates to *Comment*.