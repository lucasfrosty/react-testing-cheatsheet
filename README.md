# React testing cheatsheet
This a personal unit test cheatsheet using [React](https://facebook.github.io/react/), [Jest](https://facebook.github.io/jest/) and [Enzyme](http://airbnb.io/enzyme).  

I'm using [create-react-app](https://github.com/facebookincubator/create-react-app), so i will skip the Jest installation because that's already installed by default. I also use yarn instead of npm, so use the [Migrating from npm to yarn](https://yarnpkg.com/lang/en/docs/migrating-from-npm/) compare table to get the equivalent commands.


# Installation
To install, run the following command (for React >= 15.5):
```
yarn add enzyme react-test-renderer --dev
```

# Usage

## Shallow Rendering
- The *expect* is provide by Jest (no import necessary because it comes by default with create-react-app), see the [docs](https://facebook.github.io/jest/docs/expect.html) for more information.
- The *shallow* rendering is provide by Enzyme, see the [docs](http://airbnb.io/enzyme/docs/api/shallow.html) for more information.
```js
import React from 'react';
import { shallow } from 'enzyme';

// usually you will import this component
const CoolComponent = props => (
  <div>
    <h1>First H1</h1>
    <h1>Second H1</h1>
    <h1>Third H1</h1>
    
    <p>{props.text}</p>
  </div>
);

describe('<CoolComponent />', () => {

  it('should render three h1 elements', () => {
    const wrapper = shallow(<CoolComponent />);

    expect(wrapper.find(Foo)).toHaveLength(3); // true
  });


  it('should render text with the right text' () => {
    const wrapper = shallow(<CoolComponent text='lorem ipsum docat gep fit'/>);
    const actual = wrapper.getNode();

    expect(actual).toContain('<p>lorem ipsum docat gep fit</p>'); // TODO: test it
  });

});
```