<div align="center">
<br />
  <img src="https://raw.githubusercontent.com/tinyhttp/swagger/master/logo.svg" width="200px" alt="@tinyhttp/swagger" /><br /><br />

[![NPM][npm-badge]][npm-url] [![NPM][dl-badge]][npm-url] [![GitHub Workflow Status][actions-img]][github-actions] [![Coverage][cov-img]][cov-url]

</div>
<br />

[Swagger](https://swagger.io/) integration for [tinyhttp](https://github.com/tinyhttp/tinyhttp). This library allows you to easily generate documentation for your API.

## Install

```sh
pnpm i @tinyhttp/swagger
```

## Example

```js
import { App } from '@tinyhttp/app'
import { addToDocs, serveDocs } from '@tinyhttp/swagger'

// In case the value for a given field is an object, @tinyhttp/swagger only uses the type, optional or items (in case type is array)
// Other fields are ignored and are shown here only to imply that the same schema object can be used for validation by the fastest-validator package
const schema = {
  id: { type: 'number', positive: true, integer: true },
  name: { type: 'string', min: 3, max: 255 },
  status: 'boolean'
}

const app = new App()

app
  .get('/docs/:docId', addToDocs({ params: { docId: 'number' } }), (req, res) => {
    res.status(200).send('done')
  })
  .post(
    '/docs/:docId',
    addToDocs(
      {
        headers: { authorization: 'string' },
        params: { docId: 'number' },
        body: schema
      },
      ['docs']
    ),
    (req, res) => void res.status(200).send('done')
  )
  .get('/users', addToDocs({ query: { userId: { type: 'number', optional: true } } }, ['users']), (req, res) => {
    res.status(200).send('done')
  })
  .get('/:userId/:docId', addToDocs({ params: { userId: 'number', docId: 'number' } }), (req, res) => {
    res.status(200).send('done')
  })

// Only title is required. if servers and description are not provided, nothing is shown. version and prefix have default values of 0.1 and docs.
serveDocs(app, {
  title: 'example',
  version: '1.0',
  prefix: 'api-docs',
  description: 'this is an example for @tinyhttp/swagger',
  servers: ['www.host1.com/api/v1', 'api.host2.org/v1']
})
app.listen(3000)
```

[npm-badge]: https://img.shields.io/npm/v/@tinyhttp/swagger?style=for-the-badge&color=50A237&label=&logo=npm
[npm-url]: https://npmjs.com/package/@tinyhttp/swagger
[dl-badge]: https://img.shields.io/npm/dt/@tinyhttp/swagger?style=for-the-badge&color=50A237
[actions-img]: https://img.shields.io/github/actions/workflow/status/tinyhttp/swagger/ci.yml?style=for-the-badge&logo=github&label=&color=50A237
[github-actions]: https://github.com/tinyhttp/swagger/actions
[cov-img]: https://img.shields.io/coveralls/github/tinyhttp/swagger?style=for-the-badge&color=50A237&a
[cov-url]: https://coveralls.io/github/tinyhttp/swagger
