# Frequently Asked Questions

- [How do I use pseudo-selectors like `:checked` or `:last`?](#how-do-i-use-pseudo-selectors-like-checked-or-last)
- [How can I use `ReactCSSTransitionGroup` without any classes?](#how-can-i-use-reactcsstransitiongroup-without-any-classes)
- [How can I use Radium with jsbin?](#how-can-i-use-radium-with-jsbin)
- [Can I use my favourite CSS/LESS/SASS syntax?](#can-i-use-my-favourite-csslesssass-syntax)

## How do I use pseudo-selectors like `:checked` or `:last`?

Radium only provides the interactivity pseudo-selectors `:hover`, `:active`, and `:focus`. You need to use JavaScript logic to implement the others. To implement `:checked` for example:

```as
class CheckForBold extends React.Component {
  constructor() {
    super();
    this.state = {isChecked: false};
  }

  _onChange = () => {
    this.setState({isChecked: !this.state.isChecked});
  };

  render() {
    return (
      <label style={{fontWeight: this.state.isChecked ? 'bold' : 'normal'}}>
        <input
          checked={this.state.isChecked}
          onChange={this._onChange}
          type="checkbox"
        />
      {' '}Check for bold
      </label>
    );
  }
}
```

Instead of `:first` and `:last`, change behavior during array iteration. Note that the border property is broken down into parts to avoid complications as in issue #95.

```as
var droids = [
  'R2-D2',
  'C-3PO',
  'Huyang',
  'Droideka',
  'Probe Droid'
];

@Radium
class DroidList extends React.Component {
  render() {
    return (
      <ul style={{padding: 0}}>
        {droids.map((droid, index, arr) =>
          <li key={index} style={{
            borderColor: 'black',
            borderRadius: index === 0 ? '12px 12px 0 0' :
              index === (arr.length - 1) ? '0 0 12px 12px' : '',
            borderStyle: 'solid',
            borderWidth: index === (arr.length - 1) ? '1px' : '1px 1px 0 1px',
            cursor: 'pointer',
            listStyle: 'none',
            padding: 12,
            ':hover': {
              background: '#eee'
            }
          }}>
           {droid}
          </li>
        )}
      </ul>
    );
  }
}
```

## How can I use `ReactCSSTransitionGroup` without any classes?

Try out the experimental [`ReactStyleTransitionGroup`](https://github.com/adambbecker/react-style-transition-group) instead.

## How can I use Radium with jsbin?

To get the latest version, drop this into the HTML:

```html
<script src="https://rawgit.com/FormidableLabs/radium/master/dist/radium.js"></script>
```

We also recommend changing the JavaScript language to ES6/Babel.

## Can I use my favourite CSS/LESS/SASS syntax?

Yes, with the help of the [react-styling](https://github.com/halt-hammerzeit/react-styling) module, which requires [template strings](https://babeljs.io/docs/learn-es2015/#template-strings). Using react-styling you can write your styles in any syntax you like (curly braces or tabs, CSS, LESS/SASS, anything will do).

The example from the main Readme (using regular CSS syntax)

```as
<Button kind="primary">Radium Button</Button>
```

```as
@Radium
class Button extends React.Component {
  static propTypes = {
    kind: React.PropTypes.oneOf(['primary', 'warning']).isRequired
  }

  render() {
    return (
      <button style={style.button[this.props.kind]}>
        {this.props.children}
      </button>
    )
  }
}

const style = styler`
  .button {
    color: #fff;

    :hover {
      background: ${color('#0074d9').lighten(0.2).hexString()};
    }

    &.primary {
      background: #0074D9;
    }

    &.warning {
      background: #FF4136;
    }
  }
`
```

You can find a more advanced example in the [react-styling readme](https://github.com/halt-hammerzeit/react-styling#radium).