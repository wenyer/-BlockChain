### 按照github上的安装编译方式搭建好EOS环境之后，第一次启动本地节点时，出现错误  
version > 0: Block log was not setup properly with genesis information.  
    {}  
    thread-0  block_log.cpp:473 extract_genesis_state  
这是因为在eosio 4.2以后的版本，创世区块的配置（genesis.json）不再自动生成，而需要手动指定。解决办法是新建一个genesis.json文件（genesis.json文件的内容可以在program文件夹下找到，我把它放在了/apps/eos/build/programs/nodeos下）再执行./nodeos -e -p eosio --plugin eosio::chain_api_plugin --plugin eosio::history_api_plugin --genesis-json genesis.json，注意，因为我建立的genesis.json文件就在当前目录下，所以不需要写其的路径，如果不在一个目录，应当加上路径-genesis-json /path/to/genesis.json。  
如果在第一次启动之前就完成了建立genesis.json文件等操作，则执行不会有问题，会正常产生区块，但是往往我们第一次执行都是按照github上的文档的命令来的，即./nodeos -e -p eosio --plugin eosio::chain_api_plugin --plugin eosio::history_api_plugin ，没有加上genesis.json，此时报错了，然后再反应过来去新建genesis.json再启动，又会报一个新的错，大致的意思是创世区块只能在新的区块链上创建：Genesis state can only be set on a fresh blockchain，那是因为你可能已经运行过./nodes命令了,或者运行过eos私链,你需要将~/.local/share/eosio/nodeos/data目录删除掉,同事也可以删除掉~/.local/share/eosio/nodeos/config目录下的config.ini文件 , 这样当你再次运行就正常了。

