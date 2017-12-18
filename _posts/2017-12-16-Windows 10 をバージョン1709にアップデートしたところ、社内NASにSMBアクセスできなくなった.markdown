---
layout:post
title:"Windows 10 をバージョン1709にアップデートしたところ、社内NASにSMBアクセスできなくなった"
date:2017-12-16 00:00:00 -0700
tags:
- WindowsUpdate
- SMB
---
## 現象
先日、Windows10の端末にFall Creators Update（バージョン1709）を当てた所、
社内のNAS（Terastation）に繋がらなくなった。

## 原因
Microsoftの情報によると、Windows10バージョン1709以降は匿名認証がデフォルトだと無効になったようです。

■Microsoft - Guest access in SMB2 disabled by default in Windows 10 Fall Creators Update and Windows Server 2016 version 1709
https://support.microsoft.com/ja-jp/help/4046019/guest-access-smb2-disabled-by-default-in-windows-10-server-2016

そのため、アクセス制限なしに設定していた社内NASにアクセスできなくなったみたいですね。
これはNASだけでなく、Windowsのファイルサーバ等でも発生します。

ただ、全てのWindows10のエディションにこの設定がで適用されたわけではなく、
下記のエディションのみ設定されたようです。
（たしかにお隣のWindows10Proの社員はこの現象は起こっていなかった）

### 対象OS
- Windows 10 Enterprise and Windows 10 Education
- Windows Server 2016 Datacenter and Standard edition

## 対処方法
下記3つの対応が考えられます。
私は案2で対応し、無事SMBアクセスできるようになりました。
（ホントは案3で対応しないといけないのですが、暫定対応ということで・・）

- 案1 グループポリシーを変更する
    グループポリシー管理エディタを開き、次の項目を「未構成」から「有効」に変更
    `コンピュータの構成 > 管理用テンプレート > ネットワーク > Lanmanワークステーション > 安全でないゲストログオンを有効にする
`
    
- 案2 レジストリを変更する
    レジストリエディタを開き、次の項目を「dword:0」から「dword:1」に変更する。
    
`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\AllowInsecureGuestAuth`
    ※レジストリを変更時はお気をつけください！
    
- 案3 Terastationの設定を変える（=匿名ログオンをやめる）
    セキュリティ的にはこれ推奨ですが、運用ルールを変えることになるので、気力がある情シス向け。
