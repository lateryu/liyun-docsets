`require is not defined`
-
这个错误一般是这个browser.js库错误引起的， 千万不要使用npm里面的browser包，这和我们要用的browser.js完全是两码事

`Rendering components directly into document.body is discouraged`
-
    ReactDOM.render(
        <Mytitle  />,
        document.body
    );
渲染组件直接到document.body中是不被推荐的，建议把组件渲染到一个节点中

`Expected corresponding JSX closing tag for <input>`
-

    var MyComponent = React.createClass({
      handleClick: function() {
        this.refs.secondInput.focus();
      },
      render: function() {
        return (
          <div>
            <input type="text" ref="myTextInput" />
            <input type="text" ref="secondInput" name="" >
            <input type="button" value="Focus the text input" onClick={this.handleClick} />
          </div>
        );
      }
    });
ReactJS渲染组件中如果有单标签存在，结尾必须加上 / 符号，否则会报错