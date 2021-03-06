# containers

> These containers are used by `../routes` or by other containers.

A container does the data fetching for a small part of the application. It can represent a page or a fragment of a page.

A container is allowed to import the following stuff:
* `containers/*`
* `elements/*`
* `components/*`
* `modules/*` -> More specifically, just import the actions.

Don't import:
* `routes/*`
* `styles*`

Every container exposes a `getProps(stores, params)` method which can be used by the caller to fetch the `props` for the container. It's expected that `getProps` is called within dependency-tracker context (this is done by `items-store` `createContainer`).

## JSX

> We are using `.js` extensions instead of `.jsx`.

#### Convention
  * Containers should not contain any mark up, their primary/only concern should be how things work:

  ```js
    import Something from 'components/Something';
    import Sidebar from 'components/Sidebar';
    import { module as actions } from './module';

    class Container extends Component {

      render (
        <div>
          <Sidebar
            action={}
            action1={}
            action2={} />

          <Something action={} />
        </div>
      );
    }
  ```

  - Styling is not applied to containers.
  - When attributes get longer than two (> 2) begin and indent on new line:

  ```js
  render() {
    <Component attr1={} attr2={} />
  }

  render() {
    <Component
      attr1={}
      attr2={}
      attr3={} />
  }
  ```
  - Always add PropTypes to the Element, Component, or Container this acts as an API and will help with debugging.

  ```js
  import React, { Component, PropTypes } from 'react';
  import { connect } from 'react-redux';
  import ImmutablePropTypes from 'react-immutable-props';

  class IamComponent extends Component {
    static propTypes = {
      action: PropTypes.func.isRequired,
      anotherAction: PropTypes.func.isRequired,
      state: PropTypes.mapContains({
        key: PropTypes.string.isRequired
      }).isRequired
    };

    render() {

    }
  }
  ```

  - Keep react components dumb and testable by mapping state to props, as well as actions to props -- this allows for proper propagation of actions through `this.props` (letting redux drive).

  ```js
  import React, { Component, PropTypes } from 'react';
  import { connect } from 'react-redux';

  // ...
  const stateToProps = state => {
    // explicitly define which state keys this container needs
    return {
      vm: state.vm,
      workspace: state.vm,
    }
  };

  export default connect(stateToProps, actions)(Container);
  ```
