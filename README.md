# WxHook

## 简介

WxHook是一个基于dll注入实现的python微信机器人框架，支持多种接口、高扩展性、多线程消息处理，让你轻松应对海量消息，为你的需求实现提供便捷灵活的支持。

支持的接口
1. hook同步消息
2. 取消hook同步消息
3. hook日志
4. 取消hook日志
5. 检查登录状态
6. 获取用户信息
7. 发送文本消息
8. 发送图片消息
9. 发送文件消息
10. 发送表情消息
11. 发送小程序消息
12. 发送群@消息
13. 发送拍一拍消息
14. 获取联系人列表
15. 获取联系人详情
16. 创建群聊
17. 退出群聊
18. 获取群详情
19. 获取群成员列表
20. 添加群成员
21. 删除群成员
22. 邀请群成员
23. 修改群成员昵称
24. 设置群置顶消息
25. 移除群置顶消息
26. 转发消息
27. 获取朋友圈首页
28. 获取朋友圈下一页
29. 收藏消息
30. 收藏图片
31. 下载附件
32. 转发公众号消息
33. 转发公众号消息通过消息ID
34. 解码图片
35. 获取语音通过消息ID
36. 图片文本识别
37. 获取数据库句柄
38. 执行SQL命令
39. 测试
  
## 微信版本下载
- [WeChatSetup3.9.5.81.exe](https://github.com/tom-snow/wechat-windows-versions/releases/download/v3.9.5.81/WeChatSetup-3.9.5.81.exe)

## 安装

```bash
pip install wxhook
```
# WxHook API 文档

## 目录

1. [初始化](#初始化)
2. [事件处理](#事件处理)
3. [API 方法](#api-方法)
   - [消息发送](#消息发送)
   - [联系人和群聊](#联系人和群聊)
   - [朋友圈](#朋友圈)
   - [其他功能](#其他功能)

## 初始化

### Bot 类

```python
class Bot:
    def __init__(
        self,
        on_login: Optional[Callable[["Bot", Event], Any]] = None,
        on_before_message: Optional[Callable[["Bot", Event], Any]] = None,
        on_after_message: Optional[Callable[["Bot", Event], Any]] = None,
        on_start: Optional[Callable[["Bot"], Any]] = None,
        on_stop: Optional[Callable[["Bot"], Any]] = None,
        faked_version: Optional[str] = None
    ):
        # ...
```

初始化 Bot 实例，可以设置各种回调函数和微信版本伪装。

## 事件处理

### handle 装饰器

```python
def handle(self, events: Union[List[str], str, None] = None, once: bool = False) -> Callable[[Callable], None]:
    # ...
```

用于注册事件处理函数的装饰器。

示例：
```python
@bot.handle(events.TEXT_MESSAGE)
def on_message(bot: Bot, event: Event):
    # 处理文本消息
```

## API 方法

### 消息发送

#### send_text

发送文本消息。

- URL: `/api/sendTextMsg`
- 方法: POST
- 参数:
  - wxid: 接收者的微信ID
  - msg: 要发送的文本消息

```python
def send_text(self, wxid: str, msg: str) -> Response:
    # ...
```

#### send_image

发送图片消息。

- URL: `/api/sendImagesMsg`
- 方法: POST
- 参数:
  - wxid: 接收者的微信ID
  - imagePath: 图片文件的路径

```python
def send_image(self, wxid: str, image_path: str) -> Response:
    # ...
```

#### send_emotion

发送表情消息。

- URL: `/api/sendCustomEmotion`
- 方法: POST
- 参数:
  - wxid: 接收者的微信ID
  - filePath: 表情文件的路径

```python
def send_emotion(self, wxid: str, file_path: str) -> Response:
    # ...
```

#### send_file

发送文件消息。

- URL: `/api/sendFileMsg`
- 方法: POST
- 参数:
  - wxid: 接收者的微信ID
  - filePath: 文件的路径

```python
def send_file(self, wxid: str, file_path: str) -> Response:
    # ...
```

#### send_applet

发送小程序消息。

- URL: `/api/sendApplet`
- 方法: POST
- 参数:
  - wxid: 接收者的微信ID
  - waidConcat: 小程序联系人
  - waid: 小程序ID
  - appletWxid: 小程序的微信ID
  - jsonParam: JSON参数
  - headImgUrl: 头像图片URL
  - mainImg: 主图片
  - indexPage: 索引页面

```python
def send_applet(
    self,
    wxid: str,
    waid_contact: str,
    waid: str,
    applet_wxid: str,
    json_param: str,
    head_img_url: str,
    main_img: str,
    index_page: str
) -> Response:
    # ...
```

#### send_room_at

发送群@消息。

- URL: `/api/sendAtText`
- 方法: POST
- 参数:
  - chatRoomId: 群聊ID
  - wxids: 要@的用户ID列表，用逗号分隔
  - msg: 要发送的消息内容

```python
def send_room_at(self, room_id: str, wxids: List[str], msg: str) -> Response:
    # ...
```

#### send_pat

发送拍一拍消息。

- URL: `/api/sendPatMsg`
- 方法: POST
- 参数:
  - receiver: 群聊ID
  - wxid: 要拍一拍的用户ID

```python
def send_pat(self, room_id: str, wxid: str) -> Response:
    # ...
```

### 联系人和群聊

#### get_contacts

获取联系人列表。

- URL: `/api/getContactList`
- 方法: POST

```python
def get_contacts(self) -> List[Contact]:
    # ...
```

#### get_contact

获取联系人详情。

- URL: `/api/getContactProfile`
- 方法: POST
- 参数:
  - wxid: 联系人的微信ID

```python
def get_contact(self, wxid: str) -> ContactDetail:
    # ...
```

#### create_room

创建群聊。

- URL: `/api/createChatRoom`
- 方法: POST
- 参数:
  - memberIds: 群成员ID列表，用逗号分隔

```python
def create_room(self, member_ids: List[str]) -> Response:
    # ...
```

#### quit_room

退出群聊。

- URL: `/api/quitChatRoom`
- 方法: POST
- 参数:
  - chatRoomId: 群聊ID

```python
def quit_room(self, room_id: str) -> Response:
    # ...
```

#### get_room

获取群详情。

- URL: `/api/getChatRoomDetailInfo`
- 方法: POST
- 参数:
  - chatRoomId: 群聊ID

```python
def get_room(self, room_id: str) -> Room:
    # ...
```

#### get_room_members

获取群成员列表。

- URL: `/api/getMemberFromChatRoom`
- 方法: POST
- 参数:
  - chatRoomId: 群聊ID

```python
def get_room_members(self, room_id: str) -> RoomMembers:
    # ...
```

#### add_room_member

添加群成员。

- URL: `/api/addMemberToChatRoom`
- 方法: POST
- 参数:
  - chatRoomId: 群聊ID
  - memberIds: 要添加的成员ID列表，用逗号分隔

```python
def add_room_member(self, room_id: str, member_ids: List[str]) -> Response:
    # ...
```

#### delete_room_member

删除群成员。

- URL: `/api/delMemberFromChatRoom`
- 方法: POST
- 参数:
  - chatRoomId: 群聊ID
  - memberIds: 要删除的成员ID列表，用逗号分隔

```python
def delete_room_member(self, room_id: str, member_ids: List[str]) -> Response:
    # ...
```

#### invite_room_member

邀请群成员。

- URL: `/api/InviteMemberToChatRoom`
- 方法: POST
- 参数:
  - chatRoomId: 群聊ID
  - memberIds: 要邀请的成员ID列表，用逗号分隔

```python
def invite_room_member(self, room_id: str, member_ids: List[str]) -> Response:
    # ...
```

#### modify_member_nickname

修改群成员昵称。

- URL: `/api/modifyNickname`
- 方法: POST
- 参数:
  - chatRoomId: 群聊ID
  - wxid: 要修改昵称的成员ID
  - nickName: 新昵称

```python
def modify_member_nickname(self, room_id: str, wxid: str, nickname: str) -> Response:
    # ...
```

### 朋友圈

#### get_sns_first_page

获取朋友圈首页。

- URL: `/api/getSNSFirstPage`
- 方法: POST

```python
def get_sns_first_page(self) -> Response:
    # ...
```

#### get_sns_next_page

获取朋友圈下一页。

- URL: `/api/getSNSNextPage`
- 方法: POST
- 参数:
  - snsId: 朋友圈ID

```python
def get_sns_next_page(self, sns_id: int) -> Response:
    # ...
```

### 其他功能

#### collect_msg

收藏消息。

- URL: `/api/addFavFromMsg`
- 方法: POST
- 参数:
  - msgId: 消息ID

```python
def collect_msg(self, msg_id: int) -> Response:
    # ...
```

#### collect_image

收藏图片。

- URL: `/api/addFavFromImage`
- 方法: POST
- 参数:
  - wxid: 图片所属的微信ID
  - imagePath: 图片路径

```python
def collect_image(self, wxid: str, image_path: str) -> Response:
    # ...
```

#### download_attachment

下载附件。

- URL: `/api/downloadAttach`
- 方法: POST
- 参数:
  - msgId: 消息ID

```python
def download_attachment(self, msg_id: int) -> Response:
    # ...
```

#### forward_public_msg

转发公众号消息。

- URL: `/api/forwardPublicMsg`
- 方法: POST
- 参数:
  - wxid: 接收者的微信ID
  - appName: 应用名称
  - userName: 用户名
  - title: 标题
  - url: URL
  - thumbUrl: 缩略图URL
  - digest: 摘要

```python
def forward_public_msg(
    self,
    wxid: str,
    app_name: str,
    username: str,
    title: str,
    url: str,
    thumb_url: str,
    digest: str
) -> Response:
    # ...
```

#### ocr

图片文本识别。

- URL: `/api/ocr`
- 方法: POST
- 参数:
  - imagePath: 图片路径

```python
def ocr(self, image_path: str) -> Response:
    # ...
```

#### get_db_info

获取数据库句柄。

- URL: `/api/getDBInfo`
- 方法: POST

```python
def get_db_info(self) -> List[DB]:
    # ...
```

#### exec_sql

执行SQL命令。

- URL: `/api/execSql`
- 方法: POST
- 参数:
  - dbHandle: 数据库句柄
  - sql: SQL语句

```python
def exec_sql(self, db_handle: int, sql: str) -> Response:
    # ...
```

## 使用示例

```python
# import os
# os.environ["WXHOOK_LOG_LEVEL"] = "INFO" # 修改日志输出级别
# os.environ["WXHOOK_LOG_FORMAT"] = "<green>{time:YYYY-MM-DD HH:mm:ss}</green> | <level>{message}</level>" # 修改日志输出格式
from wxhook import Bot
from wxhook import events
from wxhook.model import Event


def on_login(bot: Bot, event: Event):
    print("登录成功之后会触发这个函数")


def on_start(bot: Bot):
    print("微信客户端打开之后会触发这个函数")


def on_stop(bot: Bot):
    print("关闭微信客户端之前会触发这个函数")


def on_before_message(bot: Bot, event: Event):
    print("消息事件处理之前")


def on_after_message(bot: Bot, event: Event):
    print("消息事件处理之后")


bot = Bot(
    faked_version="3.9.10.19", # 解除微信低版本限制
    on_login=on_login,
    on_start=on_start,
    on_stop=on_stop,
    on_before_message=on_before_message,
    on_after_message=on_after_message
)


# 消息回调地址
# bot.set_webhook_url("http://127.0.0.1:8000")

@bot.handle(events.TEXT_MESSAGE)
def on_message(bot: Bot, event: Event):
    bot.send_text("filehelper", "hello world!")


bot.run()
```
