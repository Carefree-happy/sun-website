---
sidebar_position: 5
---

axios

request config
{
    url: '/user',
    method: 'get',
    baseURL: 'http://some-domin.com/api',
    transformRequest: [function (data, headers) {
        数据发送前预处理；
        return data;
    }],
    transformResponse: [function (data) {
        数据接收后预处理；
        return data;
    }],
    headers: {'X-Requested-With': 'XMLHttpRequest'},

    params: {
        ID: 12345
    },

    paramsSerilizer: {
        完全不知道这是在干啥
    },
    data: {
        firstName: 'Fred'
    },

    data: 'Country=Brasil&City=Belo Horizonte',
    timeout: 1000,

    withCredential: false;
    adapter: function (config) {

    },
    auth: {
        username: 'janedoe',
        password: 'soopers3cret'
    },
    responseType: 'json',
}