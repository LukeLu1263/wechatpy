回复
====

目前 wechatpy 支持以下几种回复类型：`TextReply`, `ImageReply`, `VoiceReply`, `VideoReply`, `MusicReply`, `ArticlesReply` 和 `TransferCustomerServiceReply`

公共属性
----------

每一种类型的回复都有如下属性：

======= ===============================
name    value
======= ===============================
type    回复类型
source  回复的来源用户，通常是发送回复的用户。
target  回复的目标用户
time    回复的发送时间，一个 UNIX 时间戳
======= ===============================

每一种类型的回复都有一个 ``render`` 方法将回复转换成 XML 字符串 ::

    from wechatpy.replies import TextReply

    reply = TextReply()
    reply.source = 'user1'
    reply.target = 'user2'
    reply.content = 'test'

    xml = reply.render()

你可以在构建 Reply 时传入一个合法的 Message 对象来自动生成 source 和 target ::

    reply = TextReply(content='test', message=message)

TextReply 文本回复
------------------------

======= ===============================
name    value
======= ===============================
type    text
content 回复正文
======= ===============================

ImageReply 图片回复
------------------------

========= ===============================
name      value
========= ===============================
type      image
media_id  通过上传多媒体文件，得到的 id
========= ===============================

VoiceReply 语音回复
------------------------

========= ===============================
name      value
========= ===============================
type      voice
media_id  通过上传多媒体文件，得到的 id
========= ===============================

VideoReply 视频回复
------------------------

============= ===============================
name          value
============= ===============================
type          video
media_id      通过上传多媒体文件，得到的 id
title         视频回复的标题
description   视频回复的描述
============= ===============================

MusicReply 音乐回复
-----------------------

================ =======================================
name             value
================ =======================================
type             music
thumb_media_id   缩略图的媒体 id，通过上传多媒体文件，得到的 id
title            音乐回复的标题
description      音乐回复的描述
music_url        音乐链接
hq_music_url     高质量音乐链接，WiFi 环境优先使用该链接播放音乐
================ =======================================

ArticlesReply 图文回复
-------------------------

============= ===============================
name          value
============= ===============================
type          news
============= ===============================

你需要给 ArticlesReply 添加 article 来增加图文，使用 ArticlesReply 的 add_article 方法或者设置 ArticlesReply 的 articles 属性。article 应当是 dict like 的对象（只要其能响应和 dict 一致的 get 方法即可），其应当包含如下属性(键值):

============= ===============================
name          value
============= ===============================
title         图文回复标题
description   图文回复描述
image         图片链接
url           点击图文消息跳转链接
============= ===============================

使用示例 ::

    from wechatpy.replies import ArticlesReply
    from wechatpy.utils import ObjectDict

    reply = ArticlesReply(message=message)
    # simply use dict as article
    reply.add_article({
        'title': 'test',
        'description': 'test',
        'image': 'image url',
        'url': 'url'
    })
    # or you can use ObjectDict
    article = ObjectDict()
    article.title = 'test'
    article.description = 'test'
    article.image = 'image url'
    article.url = 'url'
    reply.add_article(article)


TransferCustomerServiceReply 将消息转发到多客服
-----------------------------------------------

============= ===============================
name          value
============= ===============================
type          transfer_customer_service
============= ===============================

快速构建回复
-------------

wechatpy 提供了一个便捷的 create_reply 函数用来快速构建回复 ::

    from wechatpy import create_reply

    text_reply = create_reply('text reply', message=message)

    articles = [
        {
            'title': 'test',
            'description': 'test',
            'image': 'image url',
            'url': 'url'
        },
        # add more ...
    ]

    articles_reply = create_reply(articles, message=message)
