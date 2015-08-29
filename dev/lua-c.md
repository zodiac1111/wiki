# lua和c的调用

## lua 调用 c

参考

* http://codingnow.cn/language/1530.html
* http://cn.cocos2d-x.org/tutorial/show?id=1223
* https://nicolasgoles.com/blog/2013/01/lua-tutorial-c-bindings/
* [github例子](https://github.com/zodiac1111/c_call_lua)

开关操作

	打开
	lua_State* L = luaL_newstate(); /* 创建Lua接口指针 */
	加载
	luaL_loadfile(L, fname);
	加载库?
	luaL_openlibs(L);
	执行
	lua_pcall(L, 0, 0, 0);
	关闭
	lua_close(L);

栈

	lua_pushinteger(L, *w);     //入栈1
	

获取栈内给定位置的元素值

	<TYPE> lua_to<TYPE>(lua_State * L, int index);
	lua_pop(L, 1);     // 出栈


c从lua取值

	lua_getglobal(L, "width");
	lua_getfield(L, -1, "name");

函数调用,c,lua

	lua_pushcfunction(l, l_sin);  /// 调用?
	lua_call(L, 2, 1);     // 调用lua函数


各种判断

	lua_isnumber(L, -1)
	lua_istable(L, SECOND_RET)

转换为原来的类型

	lua_tostring(L, -1)
	lua_tointeger(L, -1)

