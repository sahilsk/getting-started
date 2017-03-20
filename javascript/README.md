# Getting-Started-With-javascript


## Introduction

https://github.com/NickNaso/yahc/blob/master/lib/yahc.js#L1-L18

``` javascript

/*******************************************************************************
 * Copyright (c) 2016 Nicola Del Gobbo - Mauro Doganieri 
 * Licensed under the Apache License, Version 2.0 (the "License"); you may not
 * use this file except in compliance with the License. You may obtain a copy of
 * the license at http://www.apache.org/licenses/LICENSE-2.0
 *
 * THIS CODE IS PROVIDED ON AN *AS IS* BASIS, WITHOUT WARRANTIES OR CONDITIONS
 * OF ANY KIND, EITHER EXPRESS OR IMPLIED, INCLUDING WITHOUT LIMITATION ANY
 * IMPLIED WARRANTIES OR CONDITIONS OF TITLE, FITNESS FOR A PARTICULAR PURPOSE,
 * MERCHANTABLITY OR NON-INFRINGEMENT.
 *
 * See the Apache Version 2.0 License for specific language governing
 * permissions and limitations under the License.
 *
 * Contributors - initial API implementation:
 * Nicola Del Gobbo <nicoladelgobbo@gmail.com>
 * Mauro Doganieri <mauro.doganieri@gmail.com>
 ******************************************************************************/
```

## Commenting

https://github.com/NickNaso/yahc/blob/master/lib/yahc.js#L153-L156
``` javascript
        // The 4xx class of status codes is intended for cases in which the 
        // client seems to have erred. Except when responding to a HEAD request,
        // the server SHOULD include an entity containing an explanation of the
        // error situation, and whether it is a temporary or a permanent 
        // condition. These status codes are applicable to any request method. 
        // User agents SHOULD display any included entity to the user.
        CLIENT_ERROR: {
            BAD_REQUEST: {
                CODE: 400,
                NAME: "Bad Request"
            },
```

## Documentation

https://github.com/NickNaso/yahc/blob/master/lib/yahc.js#L124-L152

``` javascript
    /**
     * @description This object represents the sets of HTTP Status code 
     * information found at
     * @see {@link http://www.ietf.org/assignments/http-status-codes/http-status-codes.xml|ietf.org}
     * @property {object} INFORMATIONAL This object represents a set of
     * key - value regard status code indicates a provisional response, 
     * consisting only of the Status-Line and optional headers, and is 
     * terminated by an empty line.
     * @property {object} SUCCESS  This object represents a set of key - value
     * regard status code indicates that the client's request was successfully
     * received, understood, and accepted.
     * @property {object} REDIRECTION This object represents a set of 
     * key - value regard status code and indicates that further action needs to
     * be taken by the user agent in order to fulfill the request.
     * @property {object} CLIENT_ERROR This object represents a set of 
     * key - value regard status code and indicates the cases in which the 
     * client seems to have erred. Except when responding to a HEAD request, the
     * server SHOULD include an entity containing an explanation of the error 
     * situation, and whether it is a temporary or a permanent condition. These 
     * status codes are applicable to any request method. User agents SHOULD 
     * display any included entity to the user.
     * @property {object} SERVER_ERROR This object represents a set of 
     * key - value regard status code and indicates cases in which the server is
     * aware that it has erred or is incapable of performing the request. Except
     * when responding to a HEAD request, the server SHOULD include an entity 
     * containing an explanation of the error situation, and whether it is a 
     * temporary or permanent condition.
     * @public
     */
         STATUS_CODE: {
        // This class of status codes indicates a provisional response, 
        // consisting only of the Status-Line and optional headers, and it's 
        // terminated by an empty line.
        INFORMATIONAL: {
            CONTINUE: {
                CODE: 100,
                NAME: "Continue"
            },
 ```
##
