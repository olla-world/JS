# Compound component

Compound components are a design pattern in React
that allows you to create reusable components that
encapsulate logic and state.

> This can make your code more modular and easier to maintain.

The compound pattern can be a powerful tool for creating reusable and maintainable React components.

Here are some of the benefits of using compound components:

- **Modular code**: Compound components can help you break down your code into smaller, more manageable pieces. This can make your code easier to understand, test, and maintain.
- **Reusable code**: Compound components can be reused in multiple places throughout your application. This can save you time and effort when developing your application.
- **Encapsulated state**: Compound components can encapsulate state within themselves. This can make your code more predictable and easier to reason about.
  If you are developing a large React application, I recommend using compound components to help you organize your code and make it more maintainable.

**EXAMPLE :**

We are going to build a Tab component here to demonstrate the
Compound component design pattern.

```JS
import React, { useState } from "react";

import Tab from "./Tab";

function App() {
  const [currentTab, setCurrentTab] = useState(0);
  const handleCurrentTab = (tabIndex) => {
    setCurrentTab(tabIndex);
  };

  return (
    <div className="App">
      <Tab value={currentTab} onChange={handleCurrentTab}>
        <Tab.Heads>
          <Tab.Item label="Tab 1" index={0} />
          <Tab.Item label="Tab 2" index={1} />
          <Tab.Item label="Tab 3" index={2} />
        </Tab.Heads>

        <Tab.ContentWrapper>
          <Tab.Content index={0}> Content 1 </Tab.Content>
          <Tab.Content index={1}> Content 2 </Tab.Content>
          <Tab.Content index={2}> Content 3 </Tab.Content>
        </Tab.ContentWrapper>
      </Tab>
    </div>
  );
}

export default App;
```

This `App.js` file contains the presentation structure of the Tab Component.

The Tab component has four children:

- Tab.Heads component : The `Tab.Heads` component is responsible for rendering the tab header.
- Tab.Item component : The `Tab.Item` component is used to render the tab label.
- Tab.ContentWrapper component : The `Tab.ContentWrapper` component is responsible for rendering the content areas.
- Tab.Content component: The `Tab.Content` component is responsible for rendering the content for a specific tab.

```JS
import React, { useContext, createContext } from "react";

const TabContext = createContext();

export default function Tab({ children, value, onChange }) {
  return (
    <TabContext.Provider value={{ value, onChange }}>
      {children}
    </TabContext.Provider>
  );
}

const Heads = ({ children }) => <div> {children} </div>;

const Item = ({ label, index, children }) => {
  const { onChange } = useContext(TabContext);
  const handleClick = () => {
    return onChange(index);
  };

  return <div onClick={handleClick}> {label} </div>;
};

const ContentWrapper = ({ children }) => <div> {children} </div>;

const Content = ({ children, index }) => {
  const { value } = useContext(TabContext);

  return index === value ? <div> {children} </div> : null;
};

Tab.Heads = Heads;
Tab.Item = Item;
Tab.ContentWrapper = ContentWrapper;
Tab.Content = Content;
```

This is our `Tab.js` file where we implement all the logical parts for this component.
The `Tab` component uses the `useContext` hook to access the context object.
The `Heads` component uses the children's prop to render the tab labels.
The `Item` component uses the label, index, and children props to render a tab label and a content area.
The `ContentWrapper` component uses the children's prop to render the content areas.
The `Content` component uses the children and index props to render the content for a specific tab.
Here we decouple our Content and Item component in such a way that we can pass any component as content or tab item.
