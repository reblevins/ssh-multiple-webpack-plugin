# ssh-webpack-plugin
Webpack SSH multiple destinations deployment plugin.

A fork of https://github.com/unadlib/ssh-webpack-plugin

-----

>The Webpack plugin is based on ssh2 and scp2.

The webpack plugin helps developers quickly and automatically deploy a project to a single or multiple production server(s), support for ssh-privateKey login, and shell execution before and after the deployment of the command.

## Install

Install the plugin with npm:
```shell
npm install ssh-multiple-webpack-plugin --save-dev
```
## Usage
Just add the plugin to your webpack config as follows:
```
var SSHWebpackPlugin = require('ssh-multiple-webpack-plugin');
var webpackConfig = {
  entry: 'index.js',
  output: {
    path: 'build',
    filename: 'app.js'
  },
  plugins: [new SSHWebpackPlugin({
        hosts: ['Array', 'Of', 'Host(s)'],
        port: 'Remote port',
        username: 'Remote username',
        password: 'Remote password', //or use privateKey login(privateKey: require('fs').readFileSync('/path/to/private/key')).
        from: 'Deploy Local path',
        to: 'Remote full path', //important: If the 'cover' of value is false,All files in this folder will be cleared before starting deployment.
  })]
};
```

### Options

#### hosts
Type: `Array`
>An array of one or more hosts for the remote servers.

#### port
Type: `String`
Default value: `'22'`
>Port to connect to on the remote server(s).

#### username
Type: `String`
>The username to connect as on the remote server(s).

#### password
Type: `String`
>Password for the username on the remote server(s).

#### to
Type: `String`
>Full path on the remote server(s) where files will be deployed.

#### from
Type: `String`
Default value: `'build'`
>Path on your local for the files you want to be deployed to the remote server(s). No trailing slash needed.

#### cover
Type: `Boolean`
Default value: `true`
**__[Important]__: If this value is false,all files in remote path folder will be cleared before starting deployment.**
>Local Deployment files will be cover to deployment directory on remote path.

#### privateKey
Type: `string`
>Path to your private key `privateKey: require('fs').readFileSync('/path/to/private/key')`

#### passphrase
Type: `String`
>Passphrase of your private key if needed.

#### before
Type: `String` or `Array`
>Commands to run on the server(s) before and before deploy directory is created. 

#### after
Type: `String` or `Array`
>Commands to run on the server(s) before and after deploy directory is created. 

#### readyTimeout
Type: `Number`
Default value: `20000`
>Default timeout (in milliseconds) to wait for the SSH handshake to complete.

#### zip
Type: `Boolean`
Default value: `true`
>Compress the build before uploading.

#### max_buffer
Type: `Number`
Default value: `200 * 1024`
>Largest amount of data allowed on stdout or stderr.

#### exclude
Type: `Array`
Default value: `[]`
>List of folders or files to exclude from build.

### Usage Examples
Add setting in `webpack.config.js`:
```
plugins: [
    new SSHWebpackPlugin({
        host: ['127.0.0.1', '127.0.0.2', '127.0.0.3'],
        port: '22',
        username: 'root',
        privateKey: require('fs').readFileSync('/Users/username/.ssh/id_rsa'),
        before: 'cd /var/www/html && mkdir subdir', // If the from path is an entire sub-directory
        from: 'dist/subdir',
        to: '/var/www/html',
    })
]
```

If you're using vue-cli, add this to the `vue.config.js` in the root of your project:
```
module.exports = {
  ...vue config...
  configureWebpack: {
    plugins: [
      new SSHWebpackPlugin({
          host: ['127.0.0.1', '127.0.0.2', '127.0.0.3'],
          port: '22',
          username: 'root',
          privateKey: require('fs').readFileSync('/Users/username/.ssh/id_rsa'),
          before: 'cd /var/www/html && mkdir subdir', // If the from path is an entire sub-directory
          from: 'dist/subdir',
          to: '/var/www/html'
      })
    ]
  }
}
```

## Release History
* 2016/10/22 - v0.1.7 - Add the 'cover' options.
* 2016/10/22 - v0.1.4 - Initial Release.
