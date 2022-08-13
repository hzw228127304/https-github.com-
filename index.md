## Welcome to Ahuang Pages

Record my FAQ
### 关于TCP Client 接收数据延迟问题
说明：此问题于22/8/13日调试015项目发现，与线体PLC通讯的时候发现对方数据实际已经更新，但是我这边数据可以正常接收，但是会延迟30-60秒才能拿到上一个最新数据，Flush也无法解决，只能通过
断开连接再重新连接拿到最新数据，但是不停的断开打开通讯不是解决问题的方法
### 答案：后发现读取数据的时候把读取长度加长就解决了这个问题，我原本是读取10个byte 我加到1024后，此问题解决

###  json获取指定key的value
        /// <summary>
        /// 从json中获取对应key的value值
        /// </summary>
        /// <param name="json字符串"></param>
        /// <param name="需要取value对应的key"></param>
        /// <returns></returns>
        public string GetJsonValue(string strJson , string key)
        {
            //测试：
            //strJson = @"{'1':{'id':{'ip':'192.168.0.1','p':34,'pass':'ff','port':80,'user':'t'}},'code':0}";
            //key = "user"
            string strResult="";
            JObject jsonObj = JObject.Parse(strJson);
            strResult = GetNestJsonValue(jsonObj.Children(), key);
            return strResult;
        }
 
        /// <summary>
        /// 迭代获取eky对应的值
        /// </summary>
        /// <param name="jToken"></param>
        /// <param name="key"></param>
        /// <returns></returns>
        public string GetNestJsonValue(JEnumerable<JToken> jToken, string key)
        {
            IEnumerator enumerator = jToken.GetEnumerator();
            while (enumerator.MoveNext())
            {
                JToken jc = (JToken)enumerator.Current;
                if (jc is JObject || ((JProperty)jc).Value is JObject)
                {
                    return GetNestJsonValue(jc.Children(), key);
                }
                else
                {
                    if (((JProperty)jc).Name == key)
                    {
                        return ((JProperty)jc).Value.ToString();
                    }
                }
            }
            return null;
        }
