Q.1 Explain what the simple List component does.
Ans. This React List Component displays a list of items, it simply receives an array of 'items' where each item containing a text property. The SingleListItem component present in the code renders the single item in the list as an li element. The isSelected prop determines whether the item is selected or not, and onClickHandler will handle the list whenever any item present in the list is clicked on. The SingleListItem component can be selected by clicking on it. When the item is selected , its background color changes from red to green. 

Q.2 What problems / warnings are there with code?
Ans. 1. In the 'WrappedSinglelistItem' function, when any user click on the list then onClick event occurs and calls the function onClickHandler.But the onClickHandler() function called instead of function call.The solution of the problem is that use the callback function to avoid unneccessary rendering.
Code with error--
```
 <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={onClickHandler(index)}
    >
      {text}
    </li>
```

Code after removing errors--
```
<li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={()=>onClickHandler(index)}
    >
      {text}
    </li>
```
2. We mistakenly defining 'setSelectedIndex' as first arg and thinking that this is the dispatcher but infact it is the value and the second arg is  actually the dispatcher.
 
 Updated component-
```
const WrappedListComponent = ({
  items,
}) => {
  const [selectedIndex,setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);
```
3.The propTypes declaration for the items prop of the WrappedListComponent is incorrect, also shapeof property doesn’t exists.
The correct code is - 
'''
WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
};
'''
No more warnings.....


Q.3 Please fix, optimize, and/or modify the component as much as you think is necessary.

Code 
```
import React, { useState, useEffect, memo } from 'react';
import PropTypes, { node } from 'prop-types';
import './List.css'

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <div className='outer_div'>
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={onClickHandler(index)}
    >
      {text}
    </li>
    </div>
  );
};

WrappedSingleListItem.propTypes = {
  index: PropTypes.number,
  isSelected: PropTypes.bool,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({
  items,
}) => {
  const [selectedIndex,setSelectedIndex] = useState();

  useEffect(() => {
    setSelectedIndex(null);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(index);
  };

  return (
    <div className='inner_div'>
        <h1>Displaying the List items</h1>
    <ul style={{ textAlign: 'center' }}>
      {items.map((item, index) => (
        <SingleListItem
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
        />
      ))}
    </ul>
    </div>
  )
};

WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
};

WrappedListComponent.defaultProps = {
  items: [
    {text : "Item1"},
    {text : "Item2"},
    {text : "Item3"},
    {text: "Item4"},
    {text: "Item5"},
    {text: "Item6"}
  ]
};

const List = memo(WrappedListComponent);

export default List;
```

