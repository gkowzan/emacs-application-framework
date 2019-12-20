# What is Emacs Application Framework?
Emacs Application Framework (EAF) is an application framework that extend GNU Emacs to an entire new universe of powerful GUI PyQt applications.

EAF is also super extensible, developers can develop any PyQt program, and integrate it into Emacs through EAF.

This framework mainly implements three functions:
1. Integrate PyQt program window into Emacs Frame using Xlib Reparent technology
2. Listening to EAF buffer's keyboard event flow and controlling the keyboard input of PyQt program via DBus IPC
3. Created a window compositer to make the PyQt program window adapt Emacs's Window/Buffer design

## Some Screenshots

| Browser                                          | Markdown Previewer                                          |
| :--------:                                       | :----:                                                      |
| <img src="./screenshot/browser.gif" width="400"> | <img src="./screenshot/markdown_previewer.gif" width="400"> |

| Image Viewer                                          | Video Player                                          |
| :--------:                                            | :----:                                                |
| <img src="./screenshot/image_viewer.gif" width="400"> | <img src="./screenshot/video_player.gif" width="400"> |
|                                                       |                                                       |

| PDF Viewer                                          | Camera                                          |
| :--------:                                          | :----:                                          |
| <img src="./screenshot/pdf_viewer.gif" width="400"> | <img src="./screenshot/camera.gif" width="400"> |
|                                                     |                                                 |

| File Sender                                            | File Receiver                                          |
| :--------:                                             | :----:                                                 |
| <img src="./screenshot/file_transfer.png" width="400"> | <img src="./screenshot/file_uploader.png" width="400"> |
|                                                        |                                                        |


| Air Share                                          | Org Previewer                                          |
| :--------:                                         | :--------:                                             |
| <img src="./screenshot/air_share.png" width="400"> | <img src="./screenshot/org_previewer.gif" width="400"> |
|                                                    |                                                        |

| Terminal Emulator                                 |
| :--------:                                        |
| <img src="./screenshot/terminal.png" width="400"> |
|                                                   |

## Getting Started
Please read the [Wiki](https://github.com/manateelazycat/emacs-application-framework/wiki) for instructions on how to install and setup EAF.

## Launch EAF Applications
| Application Name   | Launch                                        |
| :--------          | :----                                         |
| Browser            | Type 'eaf-browser' RET https://www.google.com |
| PDF Viewer         | Type 'eaf-open' RET pdf filepath              |
| Video Player       | Type 'eaf-open' RET video filepath            |
| Image Viewer       | Type 'eaf-open' RET image filepath            |
| Markdown previewer | Type 'eaf-open' RET markdown filepath         |
| Org file previewer | Type 'eaf-open' RET org filepath              |
| Camera             | Type 'eaf-open-camera'                        |
| Terminal           | Type 'eaf-open-terminal'                      |
| File Sender        | Type 'eaf-file-sender-qrcode'                 |
|                    | Or use 'eaf-file-sender-qrcode-in-dired'      |
| File Receiver      | Type 'eaf-file-receiver-qrcode'               |
| Airshare           | Type 'eaf-file-transfer-airshare'             |
| Demo               | Type 'eaf-open-demo'                          |

To run `eaf-open` on the current file under the cursor in `dired`, call `eaf-open-this-from-dired`.

```
NOTE:
EAF use DBus' session bus, it must run in general user.
Please don't run EAF with root user, root user just can access DBus's system bus.
```

## FAQ and Support

### Read the [Wiki](https://github.com/manateelazycat/emacs-application-framework/wiki) First
For any installation and configuration assistance, please read the [Wiki](https://github.com/manateelazycat/emacs-application-framework/wiki) first!

### How about EXWM? What makes EAF special?
1. EAF gives you control over your program, while satisfying Emacs window design model. [EXWM](https://github.com/ch11ng/exwm) is only a tiling WM, that combines different applications together in an Emacs-like fashion. However, EXWM is unable to split the same application into two different windows while displaying different same application at the same time. For example, EAF is able to display same PDF on two different windows.
2. EAF essentially provides Emacs a secondary scripting language ([this topic had been brought up again in EmacsConf2019](https://media.emacsconf.org/2019/26.html) and [reddit](https://www.reddit.com/r/emacs/comments/e1wfoe/emacs_the_editor_for_the_next_40_years/)). Emacs Lisp doesn't render graphics very well, especially it doesn't play nicely with the browser. This is (an example of) where PyQt5 can come in handy.
3. With DBus IPC, EAF can use Python to control Emacs Lisp, conversely also true that Emacs Lisp can control Qt rendering and Python code.
4. EXWM, as a Windows Manager, does its job very well. Therefore, it doesn't have control and doesn't care at all how other program functions. For example, EXWM cannot control keyboard events of other programs. On the other hand, you can configure them in EAF either using existing features (see above) or write code to contribute to this repository.
5. From a higher point of view, EAF is using Emacs' design principles to extend GUI programs. You have the ability to control good GUI programs using Emacs keybindings. To achieve the ultimate goal: live in Emacs ;)

### Why this awesome framework doesn't works with MacOS or Windows?
There are mainly three obstacles:
1. We don't use MacOS or Windows
2. This framework need use X11 reparent to stick Qt5 window to emacs frame, but had trouble making X11 to work on MacOS.
3. Had trouble making dbus/python-dbus work on MacOS High Sierra
4. Had trouble making Qt5 QGraphicsView/QGraphicsScene work on MacOS, specifically QGraphicsVideoItem cannot work.
5. If you figure them out, PRs always welcome

### Why not support Wayland?
EAF use X11 XReparent technology to stick Qt5 window on Emacs buffer area, Wayland doesn't not support XReparent.

We recommend our users to use KDE, it's stable enough and supports X11 XReparent technology.

### Github Personal Access Tokens?
If you use EAF Markdown Previewer, you need the access to a [Personal access token](https://github.com/settings/tokens/new?scopes=), fill something in "Token description" and click button "Generate token" to get your personal token, then set token with code:

```Elisp
(setq eaf-grip-token "yourtokencode")
```

Otherwise, github will popup "times limit" error because so many people use grip. ;)

### "undefined symbol" error
If you got "undefined symbol" error after start EAF, and you use Arch Linux, yes, it's a bug of Arch.

You need use pip install all dependences after you upgrade your Arch system, then undefine symbol error will fix.

### \*eaf* aborted

If you got ```*eaf* aborted``` error, please check buffer ```*eaf*``` first, mostly because Python library dependencies is not installed successfully.

### Proxy
If you can't access most awesome internet services like me, you can configure the proxy settings.

```Elisp
(setq eaf-http-proxy-host "127.0.0.1")
(setq eaf-http-proxy-port "1080")
```

## Report bug

If you have any problem with EAF, please use command "emacs -Q" to start Emacs without any customizations.

Then re-test your workflow. If "emacs -Q" works fine, it's must be something wrong with your emacs config file.

If the problem persists, please [report bug here](https://github.com/manateelazycat/emacs-application-framework/issues/new).

## Join Us
Do you want to make Emacs a real operating system?

Do you want to live in emacs more comfortably?

Want to create unparalleled plugins to extend emacs?

[Let's hack together!](./docs/HACKING.md)

## 打赏
如果我的作品让你的生活充满快乐, 欢迎请我喝瓶啤酒, 哈哈哈哈

<p float="left">
    <img src="./screenshot/alipay.jpg" width="188">
    <img src="./screenshot/wechat.jpg" width="200">
</p>
