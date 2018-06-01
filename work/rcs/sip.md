## aa
1. cp opus.pc,sndfile.pc /usr/lib64/pkgconfig
2. yum install speex-devel
3. yum install ldns-devel
4. yum install libedit-devel

### freeswitch 命令
- sofia loglevel all

> 1. 创建会议室，args: loopback/会议室ID
    - `originate loopback/3000 &park`
> 2. 加入会议室，args: 会议室ID 后台拨号 user/分机号@sip地址|分机号
    - `conference 3000 bgdial user/extension1@domain`
    - `conference 3000 bgdial exten`
> 3. 追加用户，主动加入会议室，主动拨打会议室电话
> 4. 查询会议室人员，args: 会议室id xml_list
    - `conference 3000 xml_list`
> 5. 删除会议室人员(使用loopback 会导致多一个主持人在会议室里，需要删除loopback会议室才会解散)，args: 会议室id kick member_id(会议室人员ID)
    - `conference 3000 kick 2`
> 6. 显示当前通话（一个人一条记录）
    - `show channels as xml`
> 7. 显示当前通话（一条通话一条记录，会议室会有多条：loopback-a、loopback-b、参会人员）
    - `show calls as xml`
