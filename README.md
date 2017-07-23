# React testing cheatsheet
This a personal unit test cheatsheet using [React](https://facebook.github.io/react/), [Jest](https://facebook.github.io/jest/) and [Enzyme](http://airbnb.io/enzyme).  

I'm using [create-react-app](https://github.com/facebookincubator/create-react-app), so i will skip the Jest installation because that's already installed by default. I also use yarn instead of npm, so use the [Migrating from npm to yarn](https://yarnpkg.com/lang/en/docs/migrating-from-npm/) compare table to get the equivalent commands.


# Installation
To install, run the following command (for React >= 15.5):
```
yarn add enzyme react-test-renderer --dev
```
Optionally you can call sinon as well
```
yarn add sinon --dev
```

# Usage
- The *expect* is provide by Jest (no import necessary because it comes by default with create-react-app), see the [docs](https://facebook.github.io/jest/docs/expect.html) for more information.
- The *shallow* rendering is provide by Enzyme, see the [docs](http://airbnb.io/enzyme/docs/api/shallow.html) for more information.
- The *mount* rendering is provide by Enzyme, see the [docs](http://airbnb.io/enzyme/docs/api/mount.html) for more information.

## Shallow Rendering

```js
import React from 'react';
import { shallow } from 'enzyme';

// usually you will import this component
const ShallowComponent = props => (
  <div>
    <h1>First H1</h1>
    <h1>Second H1</h1>
    <h1>Third H1</h1>
    
    <p>{props.text}</p>
    <p>{props.textSecondary}</p>
  </div>
);

describe('<ShallowComponent />', () => {

  it('should render three h1 elements', () => {
    const wrapper = shallow(<ShallowComponent />);

    expect(wrapper.find(Foo)).toHaveLength(3); // true
  });


  it('should render the component exactly as expected', () => {
    const expected = (
      <div>
        <h1>First H1</h1>
        <h1>Second H1</h1>
        <h1>Third H1</h1>
        
        <p>lorem ipsum docat gep fit</p>
        <p>secondary</p>
      </div>
    );

    const wrapper = shallow(<ShallowComponent text='lorem ipsum docat gep fit' textSecondary='secondary'/>);
    expect(wrapper.getNode()).toEqual(expected);
  });

});
```

## Full DOM Rendering
*"Full DOM rendering is ideal for use cases where you have components that may interact with DOM APIs, or may require the full lifecycle in order to fully test the component (i.e., componentDidMount etc.)"*  

If you want to use, enable *jsdom* on the package.json using:
```json
"test": "react-scripts test --env=jsdom",
```

```js
import React from 'react';
import { mount } from 'enzyme';
import sinon from 'sinon';

class MountComponent extends React.Component {
  componentDidMount() {
    // do something
  };

  render() {
    return <button onClick={this.props.onButtonClick}></button>
  }
}

describe('<MountComponent />', () => {
  
  it('calls componentDidMount', () => {
    sinon.spy(MountComponent.prototype, 'componentDidMount');
    const wrapper = mount(<MountComponent />);
    expect(MountComponent.prototype.componentDidMount.calledOnce).toEqual(true);    
  });


  it('simulate click events', () => {
    const onButtonClick = sinon.spy();
    const wrapper = mount(
      <MountComponent onButtonClick={onButtonClick} />
    );
    wrapper.find('button').simulate('click');
    expect(onButtonClick.calledOnce).toEqual(true);
  });

});

```