---
layout: post
title: "用Perl实现登录淘宝并获取用户数据"
---


```c
#!/usr/bin/perl

use strict;
use LWP::UserAgent;

my $argc = @ARGV;
if ($argc == 0) {
    print "Usage:\t./fetch.pl [taobao web pages url]\n";
    print "\t./fetch.pl http://i.taobao.com/my_taobao.htm > my_taobao.html\n";
    print "\t./fetch.pl http://trade.taobao.com/trade/itemlist/list_bought_items.htm > bought_list.html\n";
    exit();
}

my $url = $ARGV[0];
my $username = '******';
my $password = '******';

my $ua = LWP::UserAgent->new(cookie_jar => {},);
$ua->default_header('Referer' => 'https://login.taobao.com/member/login.jhtml');
my $res= $ua->post('https://login.taobao.com/member/login.jhtml',
                   [
                    TPL_username => $username,
                    TPL_password => $password,
                    actionForStable => 'enable_post_user_action',
                    action => 'Authenticator',
                    event_submit_do_login => 'anything',
                    
                   ],
    );

$res = $ua->get($url);
print $res->content;

````
