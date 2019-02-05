# ssh-webpack-plugin
Webpack SSH deployment plugin.

-----

>The Webpack plugin is based on ssh2 and scp2.

The webpack plugin helps developers quickly and automatically deploy project projects to the production server, support for ssh-privateKey login, and shell execution before and after the deployment of the command.

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
        host: 'Remote host',
        port: 'Remote port',
        username: 'Remote username',
        password: 'Remote password',//or use privateKey login(privateKey: require('fs').readFileSync('/path/to/private/key')).
        from: 'Deploy Local path',
        to: 'Remote full path',//important: If the 'cover' of value is false,All files in this folder will be cleared before starting deployment.
  })]
};
```

### Options

#### host
Type: `String`
>The host of the remote server.

#### port
Type: `String`
Default value: `'22'`
>Port to connect to on the remote server.

#### username
Type: `String`
>The username to connect as on the remote server.

#### password
Type: `String`
>Password for the username on the remote server.

#### to
Type: `String`
>Full path on the remote server where files will be deployed.

#### from
Type: `String`
Default value: `'build'`
>Path on your local for the files you want to be deployed to the remote server. No trailing slash needed.

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
>Commands to run on the server before and before deploy directory is created. 

#### after
Type: `String` or `Array`
>Commands to run on the server before and after deploy directory is created. 

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
        host: '10.211.55.5',
        port: '22',
        username: 'root',
        privateKey: require('fs').readFileSync('/Users/unadlib/.ssh/id_rsa'),
        before:'mkdir beforeTest',
        after:'mkdir afterTest',
        from: './build',
        to: '/root/test',
    })
]
```
## Release History
* 2016/10/22 - v0.1.7 - Add the 'cover' options.
* 2016/10/22 - v0.1.4 - Initial Release.
