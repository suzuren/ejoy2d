
szren@ubt:~/code/ejoy2d$ ./ej2d examples/ex10.lua

*example/ex10.lua
-- game --------------------------------------------------	table: 0x2931f50
+message [function: 0x24db9b0]
+on_pause [function: 0x24e2490]
+touch [function: 0x2956e70]
+update [function: 0x24dd250]
+on_resume [function: 0x29a0f40]
+drawframe [function: 0x2957140]
+handle_error [function: 0x247d1e0]
-- ej ----------------------------------------------------	table: 0x29361e0
+load_texture [function: 0x29566b0]
+start [function: 0x29301a0]
+clear [function: 0x29332c0]
+sprite [function: 0x2956640]
+define_shader [function: 0x2926ea0]
-- pack---------------------------------------------------	table: 0x2956260
+load [function: 0x2938880]
+texture [function: 0x29565b0]
+preload [function: 0x2956400]
+path [function: 0x2956330]
+export [function: 0x29565f0]
+load_raw [function: 0x29388c0]
+preload_raw [function: 0x2956560]
-- fw ----------------------------------------------------	table: 0x247d1a0
+WorkDir []
+EJOY2D_INIT [function: 0x24d7bc0]
+inject [function: 0x247d210]
-- obj----------------------------------------------------	userdata: 0x29a0c78
-- obj2 --------------------------------------------------	userdata: 0x29a2238
----------------------------------------------------------


*ejoy2d/init.lua
ej.start(game) --在上面的开始后，只是init.lua的start function

-- callback ----------------------------------------------	table: 0x2931f50
+message [function: 0x24db9b0]
+on_pause [function: 0x24e2490]
+touch [function: 0x2956e70]
+update [function: 0x24dd250]
+on_resume [function: 0x29a0f40]
+drawframe [function: 0x2957140]
+handle_error [function: 0x247d1e0]
----------------------------------------------------------

-- fw 1 --------------------------------------------------	table: 0x247d1a0
+WorkDir []
+EJOY2D_INIT [function: 0x24d7bc0]
+inject [function: 0x247d210]
----------------------------------------------------------

在最后执行了 inject 注入之后，产生更多的function。

-- fw 2 --------------------------------------------------	table: 0x247d1a0
+EJOY2D_PAUSE [function: 0x24e2490]
+EJOY2D_HANDLE_ERROR [function: 0x247d1e0]
+EJOY2D_MESSAGE [function: 0x24db9b0]
+WorkDir []
+EJOY2D_DRAWFRAME [function: 0x2957140]
+EJOY2D_UPDATE [function: 0x24dd250]
+EJOY2D_INIT [function: 0x24d7bc0]
+EJOY2D_GESTURE [function: 0x24d4b40]
+EJOY2D_RESUME [function: 0x29a0f40]
+EJOY2D_TOUCH [function: 0x24ec3d0]
+inject [function: 0x247d210]
----------------------------------------------------------



*
















