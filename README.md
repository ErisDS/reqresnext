# reqresnext
[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg)](http://standardjs.com)
[![buildStatus](https://img.shields.io/travis/antongolub/reqresnext.svg?maxAge=60000&branch=master)](https://travis-ci.org/antongolub/reqresnext)
[![Coveralls](https://img.shields.io/coveralls/antongolub/reqresnext.svg?maxAge=60000)](https://coveralls.io/github/antongolub/reqresnext)
[![dependencyStatus](https://img.shields.io/david/antongolub/reqresnext.svg?maxAge=60000)](https://david-dm.org/antongolub/reqresnext)
[![devDependencyStatus](https://img.shields.io/david/dev/antongolub/reqresnext.svg?maxAge=60000)](https://david-dm.org/antongolub/reqresnext)

Tiny helper for express middleware testing

##### Motivation
There're several nice mocking tools for express.
The best of them (imho) is [node-mocks-http](https://github.com/howardabrams/node-mocks-http) by Howard Abrams.
This lib brings constructors, that accurately reproduces logic of express classes. That's pretty cool, but sometimes you need a bit more.

Reqresnext:
1. Just uses express proto directly.
2. Exposes the only additional property to verify outgoing data — `res.body`.

##### Usage
```javascript
    import reqresnext from 'reqresnext'
 
    it('middleware does something', () => {
      const {req, res, next} = reqresnext(<ReqOptions>, <ResOptions>)
      mware(req, res, next)
 
      expect(res.statusCode).toEqual('...')
      expect(res.body).toEqual('...')
    })
```

Also you may construct req/res instances directly:
```javascript
    import {Response, Request} from 'reqresnext'

    const foo = {name: 'foo', value: 'bar', options: {}}
    const res = new Response({cookies: [foo]})
    
    expect(res.get('Set-Cookie')).toEqual('foo=bar; Path=/')
```

Supported options are described in ./src/interface.js. The main ones:
```javascript
// cookies
    const foo = {name: 'foo', value: 'bar', options: {}}
    const res = new Response({cookies: [foo]})

// headers
    const req = new Request({headers: {foo: 'bar', baz: 'qux'}})

// url
    const req = new Request({url: 'https://example.com'})
```

Additional props that does not intersect with proto are injected as is.
```javascript
    const res = new Response({foo: 'bar', baz: 1, ...})
```