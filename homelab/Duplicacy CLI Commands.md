---
created: 2024-04-12T16:26
updated: 2024-04-12T17:36
---
[Duplicacy quick-start (CLI) - Duplicacy Forum](https://forum.duplicacy.com/t/duplicacy-quick-start-cli/1101)

```
/ # /config/bin/duplicacy_linux_x64_3.2.3 info /mnt/disks/backuphdd -e
Enter the storage password:********************************
Compression level: 100
Average chunk size: 4194304
Maximum chunk size: 16777216
Minimum chunk size: 1048576
Chunk seed: 14076ce14c8bbee5b0e3871fc9fec83f1aeb5952c398caa998b96c1156ee3359
backup
creative
isos
media
unraid
/ # /config/bin/duplicacy_linux_x64_3.2.3 init -e -storage-name local media /mnt/disks/backuphdd
Enter storage password for /mnt/disks/backuphdd:********************************
The storage '/mnt/disks/backuphdd' has already been initialized
Compression level: 100
Average chunk size: 4194304
Maximum chunk size: 16777216
Minimum chunk size: 1048576
Chunk seed: 14076ce14c8bbee5b0e3871fc9fec83f1aeb5952c398caa998b96c1156ee3359
/ will be backed up to /mnt/disks/backuphdd with id media
/ # /config/bin/duplicacy_linux_x64_3.2.3 list -id media -storage local
Storage set to /mnt/disks/backuphdd
Enter storage password:********************************
Snapshot media revision 1 created at 2024-03-31 17:54 -hash
Snapshot media revision 2 created at 2024-04-01 13:29 
Snapshot media revision 3 created at 2024-04-01 19:55 
Snapshot media revision 4 created at 2024-04-01 23:17 
Snapshot media revision 5 created at 2024-04-02 23:27 
Snapshot media revision 6 created at 2024-04-04 01:29 
Snapshot media revision 7 created at 2024-04-11 02:59 
Snapshot media revision 8 created at 2024-04-12 00:30 

/backuproot/media/restore # /config/bin/duplicacy_linux_x64_3.2.3 restore -r 1 -storage local "books/*"
Storage set to /mnt/disks/backuphdd
Enter storage password:********************************
Loaded 1 include/exclude pattern(s)
Indexing /backuproot/media/restore
Parsing filter file /backuproot/media/restore/.duplicacy/filters
Loaded 0 include/exclude pattern(s)
Restoring /backuproot/media/restore to revision 1
Downloaded books/.DS_Store (6148)
Downloaded books/APE_1.3_MOBI_BookBaby.epub (16055010)
Downloaded books/O'Reilly Linux eBooks/bashcookbook.epub (3256082)
Downloaded books/O'Reilly Linux eBooks/classicshellscripting.epub (1760292)
Downloaded books/O'Reilly Linux eBooks/cybersecurityopswithbash.epub (2769819)
Downloaded books/O'Reilly Linux eBooks/effectiveawk.epub (1730616)
Downloaded books/O'Reilly Linux eBooks/greppocketref.epub (1159230)
Downloaded books/O'Reilly Linux eBooks/introducingregularexpressions.epub (4510055)
Downloaded books/O'Reilly Linux eBooks/learningthebashshell_3rdedition.epub (1565867)
Downloaded books/O'Reilly Linux eBooks/learningtheviandvimeditors_7thedition.epub (4727032)
Downloaded books/O'Reilly Linux eBooks/linuxdevicedrivers.epub (1960522)
Downloaded books/O'Reilly Linux eBooks/linuxinanutshell.epub (2872954)
Downloaded books/O'Reilly Linux eBooks/linuxobservabilitywithbpf.epub (2684881)
Downloaded books/O'Reilly Linux eBooks/linuxpocketguide3e.epub (2929032)
Downloaded books/O'Reilly Linux eBooks/linuxsystemprogramming.epub (1610506)
Downloaded books/O'Reilly Linux eBooks/masteringregularexpressions.epub (20783241)
Downloaded books/O'Reilly Linux eBooks/sedandawk.epub (1640398)
Downloaded books/O'Reilly Linux eBooks/unixpowertools.epub (3024739)
Restored /backuproot/media/restore to revision 1
Total running time: 00:00:02
/backuproot/media # /config/bin/duplicacy_linux_x64_3.2.3 prune -id media -exhaustive -exclusive
Storage set to /mnt/disks/backuphdd
Enter storage password:********************************



The chunk ff4957a2fe61cff9a0f20be1e40942bf142d7175b042c177f21fbbc21a3cb149 has been permanently removed
The chunk fe13a5379286aab982548f924c7f0d9627dbdf5725db4633ec96a028fb8a6a2a has been permanently removed
The chunk fdbdb9430a66ffee000015429fb29506d766fe1897eea2cb6036977f8975a532 has been permanently removed
The chunk fc5a4977670d7be77f95b66132c3e4f0d10d6c04d1dd16397228a6a5a82dd042 has been permanently removed
Deleted file chunks/f6/e3dfc99c8a4e95808f24336f82290f5d24571e934e058ebb8d79fb9a2001da.lkwdcetw.tmp from the storage
The chunk f10068dcb61f3e7e217580db3f88169c40e1902c8f49a02116be4c69c3dd8912 has been permanently removed
The chunk f18447ef13f5e790230c1e47ed90a95df753c46f8c6a6b40b7c82d3dd1a1d968 has been permanently removed
The chunk f080ef2e9194ed1199119d0a20f5a61b1588021db2a2c232a0de724846a4ab1a has been permanently removed
The chunk ee5c17b0cc18a2ee7f0594330afbe967cbec6f07fd1443a16acb8c5ecffa03bf has been permanently removed
The chunk eeabfe3d6d4f1ebdb2a5a24a5a57f3a3c147e1815e2ccdfb6ebf26a668f759f3 has been permanently removed
The chunk ede1b640478cbf8a027284a36e1642df74b592d84b1cfaaf2be5812fc8cc0435 has been permanently removed
The chunk ec0242b6cd09ad9e2ea53066f216ea10439019235ca0ce7c5b94b42ea3dbc82d has been permanently removed
The chunk e6e1d90a4b426d60606a84aa7b772e413407892357374210962fdfe247de34c7 has been permanently removed
The chunk e4a64518855ed40a2f9a4f8ab0817e196cf745d104a8b70ffc870bb097b32f43 has been permanently removed
The chunk e4d386579c50df1ff4a881beba7f6001dccc10fbba25387a1335f160bdda2b99 has been permanently removed
The chunk e354e8e02701fbc941d0ce1e9951020053278496dc2e88ef94383b0eff01fe92 has been permanently removed
The chunk e25ef94bf32428dbe00f9f43dacb557d2bbab415cf40778c47fc29953ea1dde9 has been permanently removed
The chunk e1387d29813e05af96dba370a67d9388fd54ccb5a64ed9cf875cc9ee17de82ca has been permanently removed
The chunk e1bc0bf03f93ac5f50b305465749ae45612c15e739e81f92eccf55f60a03f615 has been permanently removed
The chunk de1d617a1fc4a8ce560de3be6bbea6afedef09fc82d9618b64b12a3622094d68 has been permanently removed
The chunk dc5a00a326eb15b9c9e622ef10796994b50b4d8f0d9628f2c609ca19fe9bbd7f has been permanently removed
The chunk db479f5398d02f4cec7c0fb7b91dc70d22601de82b5bb20372a8971e95e84c71 has been permanently removed
The chunk d8c9bc1408a632b703f130fe26e78baa90b642d5012e81e67d2b317c47a5d5fb has been permanently removed
The chunk d56789dedef75b906550a4405bc645d67189917cec5227cb1d3aa13dd86ecea7 has been permanently removed
The chunk c7236cacceda1c21beed85cff8e8b76c26ea048e9a56176da5fbc327130bf829 has been permanently removed
The chunk c7c8d6977676bea7b72696d9f694a93ed942bab057a53e7424ab81588c5b4897 has been permanently removed
The chunk c6ae7200c229f6bec6b9dc330fdb20227b86a9f6015d5eb17fc4ce614263f3c9 has been permanently removed
The chunk c34b8de1c284b7598c640a892be19795d87dad773795f06e129084e95c9e6b6d has been permanently removed
The chunk c20c676fc03d55f481ab21694b9f8489483fc2a8c763202bcce41b1190eb1b97 has been permanently removed
The chunk c280178f437b23bbc13ecd6b8a9913fb4cde04ea17e16d68d66ddf698bd4dd81 has been permanently removed
The chunk c17db730b086cd3c7018aeb6a8ef1a4aaf5569010b567dea8719dcb9af5f13dc has been permanently removed
The chunk c0605198f3e3af60b718ede96c68585b7ce84fa23813e365a2e7a823c47d1db0 has been permanently removed
The chunk bde83bb7350ef0b2ef00927a9cc8951d1da0544bad2d1104cde172e533d9b75e has been permanently removed
The chunk bb7e321e5118fd9eca143eddc482e61bdb220e520aea7163af136595819733d9 has been permanently removed
The chunk b80c20cdb43244146e06f9858ef5d424b7928ddcbc00cac571db93e07046976f has been permanently removed
The chunk b77d97d6a9968b81b794a01e104903c92b72e62b4d4c4fc1e468bac17e945907 has been permanently removed
The chunk b62ebfd8af5cc4505f4e1142fc1adf693ca329f2fcdbfe1680b55935c73074a8 has been permanently removed
The chunk b6f1b756eb4148e7e4d739db125e93076aeecfdafe9d9d80aa805887923ea925 has been permanently removed
The chunk b3cb14b2093200c28aa039c3be218dc06f7eb343bc5063273bd26ec9bd006ede has been permanently removed
The chunk b12ed859086b1275650407c25eba76d4b36bb7ab42c323e360101f15c4bb1dc2 has been permanently removed
The chunk b1d33e4e99f471ecbdb4586e3fc122dbbaa539392a4799c0468478999a6cf703 has been permanently removed
The chunk b0df0f054e3bd2409f9cccc882313a78feb6724c30f7f86c3033bd52d6c08977 has been permanently removed
The chunk aeeb566ca51a47e51c7043887bbeb0f397257e5745280789fe0790faa57af85e has been permanently removed
The chunk ad552a556101c1b91cf82236c715d40a779ebcd1b45a968b6ee97fdf45116065 has been permanently removed
The chunk ad91edca30b51289527125612975ef5b4bce5a56afbf7916bc71ee71e2b919a2 has been permanently removed
The chunk ab1c13ead715a69a469a668b3f4b21800f3ffc0077b310616b756a4542597c7e has been permanently removed
The chunk aa2668d5f192f2b65af2107c4d6310d6bf7344562a58612a15d7d4d110c888a0 has been permanently removed
The chunk aadee3365c079c306ef1280d2bb73985548b0a572a5c3b91650c07d968853b6a has been permanently removed
The chunk aae96607dc2a92d7d591b068b547f7eed362e3e9b01d0e0c6b901169c74717d5 has been permanently removed
The chunk a9689418afe8fa714752bbd2fe0429df6c2a96439f33ed0b516f9ecafd82436b has been permanently removed
The chunk a81d4c1c20be032600415f5586fb7cb098d7a3bf3318fcd7697b76a877560971 has been permanently removed
The chunk a28080ca12177570bcbd28d4a173595237bb5c4fd0d35655bee24efe9d8d7e7e has been permanently removed
The chunk a1c4a56a426241e1d8b2509313bc21451fc3974332faebad122cfbb55be47db7 has been permanently removed
The chunk a0d83681791e677bb110ae9f223206a3df872124647df7a38d57ee0e8223b826 has been permanently removed
The chunk 9917670221f7044db772939de2723630fcd1c38da77d3733f39aa63e5e2462b6 has been permanently removed
The chunk 98172a2ad0054a6c304b9b94a33b6f74c42732ae2b94e7c22e0c1249a854fbae has been permanently removed
The chunk 9435975e77f827844ff4b62eedf01f0ce56f4a833f98cdbe4ce7c6e26868c777 has been permanently removed
The chunk 94a3501982d33491be8ec09d867815e4196b46f882ce46e0e3b6abc7115d9383 has been permanently removed
The chunk 94e5290ff27f642ee15bc6b4005e4ad828437f12b73371691fabd312ed5db2dd has been permanently removed
The chunk 8e8e4e663cc040e126f4c8bbc3df809283e19ec6ffe465b17396af36d32a5efc has been permanently removed
The chunk 8bbb5fb512908963c3edee4e37755b58fe463f22860a414b3b980ddda6eddf28 has been permanently removed
The chunk 851d1722c72cb49d9892bafd4a13fe10addfc3197b9ba48ad13fbf3a5c61a2f2 has been permanently removed
The chunk 83c683023d18c9423829ffb8f61f852974b64ad3d1789776baae09f0adef7edd has been permanently removed
The chunk 811954ee81c2c9c93723d4f09c09f9475afd8ad6ac930f3ec23bda6c11a95abe has been permanently removed
The chunk 7a50cd5c27736fc2018656ae4425011b2f33dae14323e6175f81ca4daf96d483 has been permanently removed
The chunk 7ae079856de8cea2052a100f678c436a3149464bd2224d8819ced6b3ff11dc5e has been permanently removed
The chunk 79cc09a889dff1c2bee31ce721397bad92d4a965a6e866c22dc11be185c6199e has been permanently removed
The chunk 776739bd09e0a78c06414469e70d669bc718d63986367a1877e1ef69cb622ee9 has been permanently removed
The chunk 742c9775e53b3147228a0b4d093a6082f10a4feb3be34b92825daabdc610f5cd has been permanently removed
The chunk 72958142b889ce6c9025b3022884f29088503799b903f2eb4ef44c36570ff843 has been permanently removed
The chunk 6f93fedd0a660616344507a070f9b82c9812504fe897f19fea61621199d5e04a has been permanently removed
The chunk 6dab34b9a5b80293706917a447d406771dd1ff42315420604472d1324009a006 has been permanently removed
The chunk 66153c658aee89070abe7b6bd62831210ebf02c412b58b1fedcf008d225cdafd has been permanently removed
The chunk 6511c5f7c05d9b307809588804d2a56e2d12817aac7579cb172b4668c5612b4c has been permanently removed
The chunk 6407154440b2855d22b3edcc48e9a89cf2615dc5574a8d865292ec24eaf1cc9d has been permanently removed
The chunk 6279fc96e4934b7f23db32958add43ff209b6543a5f93ee40dbc271bbac5a5f6 has been permanently removed
The chunk 6031db5ac2137c6e1437c7326454f5e127c879c8c7a3128900126b710eb95565 has been permanently removed
The chunk 5fdf1289f3b5009e83e9c0ecbd4f36fee9e460afe9148dd07bde0945684608cd has been permanently removed
The chunk 5ecd0a8195d223ce85c011940e58d5d1095f5d84f99c5ad9361d0df294ea3198 has been permanently removed
The chunk 597673d5f08f97b0915e105de8723233fd68729e1ade1966984663a80fe3b8ee has been permanently removed
The chunk 583571cdb2625d14447681638ef64ee2e4cc21da29140e45bd3ee284fbd264e2 has been permanently removed
The chunk 563c13c707564b21e35332b48116b7932b75eddec3ef38f9b76fbbe878fb2dfa has been permanently removed
The chunk 566912abdacf9c170c53eb352e4942bc6af2dc1208ac65dd9da3917b00510ce8 has been permanently removed
The chunk 52e15addb3381bcb433bc45e26696b41394e63a6437b7b2532467ad26dc95086 has been permanently removed
The chunk 519d55b042fa5b2694919029da2f830dfd76d5d1c72875fe6b8fb421673d4d5b has been permanently removed
The chunk 51fc524c8f98572a0afc61b76fb9a7c1f639d35eb2fe34e6af2af1a83471b299 has been permanently removed
The chunk 507235b3f0bbd4dbc3935d36b3a6829147b05ae8c5914665e59b2ac02450cef2 has been permanently removed
The chunk 4ce4ba718727c9a894536f91199a89b93181cb45dc1f52ca6b20e01d9162e4b2 has been permanently removed
The chunk 4acec2ed62d4bf1ee75fa690262b4ef304dd24244664a94a41398fbf617a877e has been permanently removed
The chunk 47ee60735e8c733831e6b58b80a9951454596285a7f4a57e942fb5c46392a6e7 has been permanently removed
The chunk 45d0821f20ba35e424d23c20746f92a2e404ad77702aadeaf6c19fdeef0fe76d has been permanently removed
The chunk 449d00b1e0bd6e319e336aebc5c7178f5f4fc1918c4292a3ab35a963e203f4e4 has been permanently removed
The chunk 3f3ecfba7ef379d7a1a629f3d780a94a0aeb289c76a104453da95ef156653252 has been permanently removed
The chunk 3c2211558d06c237323587a986fc34be57be9fd36927ed3098a79234bd020d03 has been permanently removed
The chunk 3624b2cb22e521de831548bb3137c77f4e2396fd807e40aa41c0558349340425 has been permanently removed
The chunk 3629b797fc1f83383d541493e970c827eff6e92d0fc67099ef33c92448d2d5dd has been permanently removed
The chunk 357c8f1276e9be3be97b2992f098796b9e41f755ca1088185f62351867b9e740 has been permanently removed
The chunk 35e9b32f911a1e8fa68c49263586333c511147bd13662109afb252994343f7fe has been permanently removed
The chunk 347761c9606b8af391e516ad42a3359e99cae89df928e62908d7d9f57599cd5c has been permanently removed
The chunk 33f1d47d6dfec23e7fda30da23585803c4dd23cc1a08a92d99cf19afc893c085 has been permanently removed
The chunk 3287d6e4101ed1dbe36b5cee191d8a64eece8271990b72260b0cc560e81c1be2 has been permanently removed
The chunk 2d256d733fd8a0bcc88050fb6e06c4c33d416e2ce7504a7f40771562d7ab658c has been permanently removed
The chunk 2c62368e92d41823aca0380759c5255f553607b1c6f4f775cdfd60e6d8d45251 has been permanently removed
The chunk 2cfadea3ece642d2fb074630f417f7ab4ddc5f476bbdfdc90692fc2cadfa18b7 has been permanently removed
The chunk 2b99e4bfa97890663d5feadc03f31c860a575384e951513120f6424eb13d8e91 has been permanently removed
The chunk 25b7370d01a5ebf6636dd7c8acd1f780591536f1ebc14b6dded42ff16c5e2eb3 has been permanently removed
The chunk 205ecf005724116e42b0fa703fd4543802e12002d3d7c947d27109bf1ee73e57 has been permanently removed
The chunk 20e67dcd9a84c21922a4d809bae94286e8b5943ded991ef844c310efe51eb6e0 has been permanently removed
The chunk 1cc86b8a42a4e7ea9e10ad5294bb5a2c466d590373600361f75c388c7d32bc84 has been permanently removed
The chunk 18051a0bda04ecdb48eb9a9723a2a1857c9edbed89f8ed5f4068d7b0ad12150e has been permanently removed
The chunk 1772e5b333226d8699d35202992ed5bb577c51c9f559cb6207e1a92b2ac5c22d has been permanently removed
The chunk 1485fca4b607f21f556b7d5f44b91b1630220a23ba4f3fc09513ae06ce0afd31 has been permanently removed
The chunk 12daeac2b41b8046613dc94afcc0248cf3f422da6aff8efff4655d282d426db7 has been permanently removed
The chunk 0f2375bd228e66007858d6b2254795c569aa2798d2371a1d3d9b43644dd8510e has been permanently removed
The chunk 0e2cee3dbf2d0e352654f1d9726a8e2dd0927e0c8936b950e72774747c864292 has been permanently removed
Deleted file chunks/0e/34594e77bc71f271e0f7c863a45de4495471332727df8954ffb9290ffa393c.qtekyvjg.tmp from the storage
The chunk 0cc9b5eee8b5da093e5ef3e6af463a667fe52a8a1671695f16c02dbf99423e1b has been permanently removed
The chunk 0b9a4c401bf33acefd0ba6d300d14f9c96b22cf80309cf77056fd7cc0322fba6 has been permanently removed
The chunk 0953e755f2b1502f67e8c340215e690903b2d0298aab3a19c228484ef053c220 has been permanently removed
The chunk 06f0de757e270e0c7640f85b514e2cddc2b3b3576167005aef92b913a913abc4 has been permanently removed
The chunk 030c3ff393b17396c84f98c84bd4064b686e975b770e158b162a41e39420125a has been permanently removed
The chunk 00e66b3a592e119da80a8dd18d256d161ea8049175b28c7f16f9c3ef3e0dd9c8 has been permanently removed
```

The repository is the local directory that contains the data you wish to backup. When you initialize Duplicacy in a specific directory, that directory becomes the repository.