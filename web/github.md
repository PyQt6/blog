
# [GitHub](https://github.com)

## 描述

全球最大的猿(程序员)类同性交友网站，又称基佬hub

大多数开源项目都在这里维护，没在这里维护的大多数也有github镜像

善用[搜索](https://github.com/search?q=)功能

- [登录](https://github.com/login)
- [注册](https://github.com/join)

用户仓库格式为
https://github.com/用户名/仓库名

## 常见问题 FAQ


### github 仓库体积限制

仅供了解，切勿滥用。希望自觉维护良好社区。

单文件
> 单个文件大于 50M 时会收到警告, 100M 时会被拒绝。

该警告将告诉您哪些文件太大：

> remote: warning: Large files detected.    
> remote: warning: File big_file is 55.00 MB; this is larger than GitHub’s recommended maximum file size of 50 MB

推送信息big_file已接收并保存到GitHub上的存储库中，但是您应该考虑完全删除文件和提交。

100 MB推送限制
> 如果您将大于100 MB的文件推送到GitHub，Git将拒绝推送并告诉您哪个文件太大：

> remote: warning: Large files detected.    
> remote: error: File giant_file is 123.00 MB; this exceeds GitHub’s file size limit of 100 MB

由于，此推送被拒绝giant_file。提交将不会保存到GitHub上的存储库中。

用户或仓库
> 用户账号下所有仓库体积总和没有限制。
> 单个仓库硬限制为100G。超过75G时会收到警告，推荐为1G以下。(如果超过1GiB你将会收到一封邮件叫你减小仓库体积)

通过浏览器将文件添加到存储库，则文件不能大于25 MB。

参考
> 我的磁盘配额是多少？ https://help.github.com/en/articles/what-is-my-disk-quota    
> 大文件的条件 https://help.github.com/en/articles/conditions-for-large-files


### 查看GitHub仓库大小的几种方法

1. GitHub自带查看自己的仓库大小的功能（public、private）

用户Settings->Repositories可以直接看到

2. 利用GitHub提供的API查看（public）

https://api.github.com/repos/user/repo ，访问后得到一个包含仓库信息的json数据。

例如[citra](https://api.github.com/repos/citra-emu/citra-canary)的
json数据如下：其中的size就是仓库大小，单位是KiB。

```json

{
  "id": 98605067,
  "node_id": "MDEwOlJlcG9zaXRvcnk5ODYwNTA2Nw==",
  "name": "citra-canary",
  "full_name": "citra-emu/citra-canary",
  "private": false,
  "owner": {
    "login": "citra-emu",
    "id": 7605670,
    "node_id": "MDEyOk9yZ2FuaXphdGlvbjc2MDU2NzA=",
    "avatar_url": "https://avatars.githubusercontent.com/u/7605670?v=4",
    "gravatar_id": "",
    "url": "https://api.github.com/users/citra-emu",
    "html_url": "https://github.com/citra-emu",
    "followers_url": "https://api.github.com/users/citra-emu/followers",
    "following_url": "https://api.github.com/users/citra-emu/following{/other_user}",
    "gists_url": "https://api.github.com/users/citra-emu/gists{/gist_id}",
    "starred_url": "https://api.github.com/users/citra-emu/starred{/owner}{/repo}",
    "subscriptions_url": "https://api.github.com/users/citra-emu/subscriptions",
    "organizations_url": "https://api.github.com/users/citra-emu/orgs",
    "repos_url": "https://api.github.com/users/citra-emu/repos",
    "events_url": "https://api.github.com/users/citra-emu/events{/privacy}",
    "received_events_url": "https://api.github.com/users/citra-emu/received_events",
    "type": "Organization",
    "site_admin": false
  },
  "html_url": "https://github.com/citra-emu/citra-canary",
  "description": "THIS IS A STAGING REPO FOR OUR CANARY RELEASES ONLY. For development see our main repo at https://github.com/citra-emu/citra",
  "fork": false,
  "url": "https://api.github.com/repos/citra-emu/citra-canary",
  "forks_url": "https://api.github.com/repos/citra-emu/citra-canary/forks",
  "keys_url": "https://api.github.com/repos/citra-emu/citra-canary/keys{/key_id}",
  "collaborators_url": "https://api.github.com/repos/citra-emu/citra-canary/collaborators{/collaborator}",
  "teams_url": "https://api.github.com/repos/citra-emu/citra-canary/teams",
  "hooks_url": "https://api.github.com/repos/citra-emu/citra-canary/hooks",
  "issue_events_url": "https://api.github.com/repos/citra-emu/citra-canary/issues/events{/number}",
  "events_url": "https://api.github.com/repos/citra-emu/citra-canary/events",
  "assignees_url": "https://api.github.com/repos/citra-emu/citra-canary/assignees{/user}",
  "branches_url": "https://api.github.com/repos/citra-emu/citra-canary/branches{/branch}",
  "tags_url": "https://api.github.com/repos/citra-emu/citra-canary/tags",
  "blobs_url": "https://api.github.com/repos/citra-emu/citra-canary/git/blobs{/sha}",
  "git_tags_url": "https://api.github.com/repos/citra-emu/citra-canary/git/tags{/sha}",
  "git_refs_url": "https://api.github.com/repos/citra-emu/citra-canary/git/refs{/sha}",
  "trees_url": "https://api.github.com/repos/citra-emu/citra-canary/git/trees{/sha}",
  "statuses_url": "https://api.github.com/repos/citra-emu/citra-canary/statuses/{sha}",
  "languages_url": "https://api.github.com/repos/citra-emu/citra-canary/languages",
  "stargazers_url": "https://api.github.com/repos/citra-emu/citra-canary/stargazers",
  "contributors_url": "https://api.github.com/repos/citra-emu/citra-canary/contributors",
  "subscribers_url": "https://api.github.com/repos/citra-emu/citra-canary/subscribers",
  "subscription_url": "https://api.github.com/repos/citra-emu/citra-canary/subscription",
  "commits_url": "https://api.github.com/repos/citra-emu/citra-canary/commits{/sha}",
  "git_commits_url": "https://api.github.com/repos/citra-emu/citra-canary/git/commits{/sha}",
  "comments_url": "https://api.github.com/repos/citra-emu/citra-canary/comments{/number}",
  "issue_comment_url": "https://api.github.com/repos/citra-emu/citra-canary/issues/comments{/number}",
  "contents_url": "https://api.github.com/repos/citra-emu/citra-canary/contents/{+path}",
  "compare_url": "https://api.github.com/repos/citra-emu/citra-canary/compare/{base}...{head}",
  "merges_url": "https://api.github.com/repos/citra-emu/citra-canary/merges",
  "archive_url": "https://api.github.com/repos/citra-emu/citra-canary/{archive_format}{/ref}",
  "downloads_url": "https://api.github.com/repos/citra-emu/citra-canary/downloads",
  "issues_url": "https://api.github.com/repos/citra-emu/citra-canary/issues{/number}",
  "pulls_url": "https://api.github.com/repos/citra-emu/citra-canary/pulls{/number}",
  "milestones_url": "https://api.github.com/repos/citra-emu/citra-canary/milestones{/number}",
  "notifications_url": "https://api.github.com/repos/citra-emu/citra-canary/notifications{?since,all,participating}",
  "labels_url": "https://api.github.com/repos/citra-emu/citra-canary/labels{/name}",
  "releases_url": "https://api.github.com/repos/citra-emu/citra-canary/releases{/id}",
  "deployments_url": "https://api.github.com/repos/citra-emu/citra-canary/deployments",
  "created_at": "2017-07-28T03:39:17Z",
  "updated_at": "2021-06-05T09:02:09Z",
  "pushed_at": "2021-05-16T08:45:15Z",
  "git_url": "git://github.com/citra-emu/citra-canary.git",
  "ssh_url": "git@github.com:citra-emu/citra-canary.git",
  "clone_url": "https://github.com/citra-emu/citra-canary.git",
  "svn_url": "https://github.com/citra-emu/citra-canary",
  "homepage": "",
  "size": 43835,
  "stargazers_count": 239,
  "watchers_count": 239,
  "language": "C++",
  "has_issues": false,
  "has_projects": false,
  "has_downloads": true,
  "has_wiki": false,
  "has_pages": false,
  "forks_count": 70,
  "mirror_url": null,
  "archived": false,
  "disabled": false,
  "open_issues_count": 1,
  "license": {
    "key": "gpl-2.0",
    "name": "GNU General Public License v2.0",
    "spdx_id": "GPL-2.0",
    "url": "https://api.github.com/licenses/gpl-2.0",
    "node_id": "MDc6TGljZW5zZTg="
  },
  "forks": 70,
  "open_issues": 1,
  "watchers": 239,
  "default_branch": "master",
  "temp_clone_token": null,
  "organization": {
    "login": "citra-emu",
    "id": 7605670,
    "node_id": "MDEyOk9yZ2FuaXphdGlvbjc2MDU2NzA=",
    "avatar_url": "https://avatars.githubusercontent.com/u/7605670?v=4",
    "gravatar_id": "",
    "url": "https://api.github.com/users/citra-emu",
    "html_url": "https://github.com/citra-emu",
    "followers_url": "https://api.github.com/users/citra-emu/followers",
    "following_url": "https://api.github.com/users/citra-emu/following{/other_user}",
    "gists_url": "https://api.github.com/users/citra-emu/gists{/gist_id}",
    "starred_url": "https://api.github.com/users/citra-emu/starred{/owner}{/repo}",
    "subscriptions_url": "https://api.github.com/users/citra-emu/subscriptions",
    "organizations_url": "https://api.github.com/users/citra-emu/orgs",
    "repos_url": "https://api.github.com/users/citra-emu/repos",
    "events_url": "https://api.github.com/users/citra-emu/events{/privacy}",
    "received_events_url": "https://api.github.com/users/citra-emu/received_events",
    "type": "Organization",
    "site_admin": false
  },
  "network_count": 70,
  "subscribers_count": 28
}

```

3. 利用浏览器插件：Enhanced GitHub（public、private）

该插件在GitHub上开源：https://github.com/softvar/enhanced-github

功能
> 显示仓库大小，每个文件的大小，下载链接和复制文件内容的选项，在GitHub网站顶部提供有用功能的扩展。

注意：私有仓库需要添加Token
该插件适用于公开和私有仓库，但是私有仓库需要添加token，不然因为权限不够，会访问不了。

添加token步骤：（详情可见：https://github.com/softvar/enhanced-github#github-api-rate-limiting）

   1. 先到 https://github.com/settings/tokens ，点击“Generate new token”，范围勾选repo就可以了(如下)，拉到最下面，点击“Generatetoken”申请一个token。

   2. 复制token，然后（右键）点击该插件，选择“选项”，跳转后，等一会儿（10s左右），会弹出如下弹窗，将复制的token填进去，然后去访问一个自己的私有仓库就可以显示大小。
 

补充：该插件的另一个很棒的功能就是很简单就能下载一个文件（暂时不支持下载文件夹）



### github下载慢

github的releases上的文件是托管在amazon上的，因此有时会被墙，导致下载过慢或停止。

github下载慢会导致下载失败，因为它的链接有时效，超时后链接会过期，必须在时限内下完。

#### 方法1(适用于下载整个压缩包)

到 https://d.serctl.com/ 输入下载链接即可下载。注意，所有人都能看到你下载的东西，也可以下载你要下载的东西。因此不要用此方式下载包含个人信息的内容。其他网站下载慢的也可以把下载链接放到这里。

原理：先帮你转存，你再从他的服务器上下载。

或者可以使用其他人提供的浏览器插件，原理应该差不多。

附注：亲测2020年以后github的东西基本下载较快，如果不是大文件(>512MiB)请直接下载，不要经常占用服务器资源，服务器倒了的话自己也没得用。

#### 方法2(适用于下载仓库中的单个文件)

https://cdn.jsdelivr.net/gh/用户名/仓库名/文件在仓库内的路径

比如

https://cdn.jsdelivr.net/gh/pyqt6/blog/Changelog.txt

用户名、仓库名不区分大小写  
文件名区分大小写，写错(比如写成changelog.txt)会：    
Couldn't find the requested file /changelog.txt in pyqt6/blog.

又比如https://github.com/zhangjt/fileOnline中的06.zip(话说谁知道这个压缩包的密码？)   
https://cdn.jsdelivr.net/gh/zhangjt/fileOnline/lightNote/zhandouyuanpaiqianzhong/06.zip

找不到文件会：  
Failed to fetch zhangjt/fileOnline@latest from GitHub.







