jquery-databind
===============

Inject Knockout data-bind attributes without cluttering source HTML

Usage
-----

Turn this:
```html
<form data-bind="submit:addItem">
    Add item: <input type="text" data-bind='value:itemToAdd, valueUpdate: "afterkeydown"' />
    <button type="submit" data-bind="enable: itemToAdd().length > 0">Add</button>
</form>
 
<p>Your values:</p>
<select multiple="multiple" height="5" data-bind="options:allItems, selectedOptions:selectedItems"> </select>
 
<div>
    <button data-bind="click: removeSelected, enable: selectedItems().length > 0">Remove</button>
    <button data-bind="click: sortItems, enable: allItems().length > 1">Sort</button>
</div>
```
and this:
```javascript
var BetterListModel = function () {
    this.itemToAdd = ko.observable("");
    this.allItems = ko.observableArray(["Fries", "Eggs Benedict", "Ham", "Cheese"]); // Initial items
    this.selectedItems = ko.observableArray(["Ham"]);                                // Initial selection
 
    this.addItem = function () {
        if ((this.itemToAdd() != "") && (this.allItems.indexOf(this.itemToAdd()) < 0)) // Prevent blanks and duplicates
            this.allItems.push(this.itemToAdd());
        this.itemToAdd(""); // Clear the text box
    };
 
    this.removeSelected = function () {
        this.allItems.removeAll(this.selectedItems());
        this.selectedItems([]); // Clear selection
    };
 
    this.sortItems = function() {
        this.allItems.sort();
    };
};
 
ko.applyBindings(new BetterListModel());
```
to this:
```html
<form>
    Add item: <input type="text" />
    <button type="submit">Add</button>
</form>
 
<p>Your values:</p>
<select multiple="multiple" height="5"> </select>
 
<div>
    <button class="remove">Remove</button>
    <button class="sort">Sort</button>
</div>
```
and this:
```javascript
var BetterListModel = function () {
    this.itemToAdd = ko.observable("");
    this.allItems = ko.observableArray(["Fries", "Eggs Benedict", "Ham", "Cheese"]); // Initial items
    this.selectedItems = ko.observableArray(["Ham"]);                                // Initial selection
 
    this.addItem = function () {
        if ((this.itemToAdd() != "") && (this.allItems.indexOf(this.itemToAdd()) < 0)) // Prevent blanks and duplicates
            this.allItems.push(this.itemToAdd());
        this.itemToAdd(""); // Clear the text box
    };
 
    this.removeSelected = function () {
        this.allItems.removeAll(this.selectedItems());
        this.selectedItems([]); // Clear selection
    };
 
    this.sortItems = function() {
        this.allItems.sort();
    };
};

$('form').databind({
    submit: 'addItem'
});

$('form > button').databind({
    enable: 'itemToAdd().length > 0'
});

$('select').databind({
    options: 'allItems',
    selectedOptions: 'selectedItems'
});

$('button.remove').databind({
    click: 'removeSelected',
    enable: 'selectedItems().length > 0'
});

$('button.sort').databind({
    click: 'sortItems',
    enable: 'allItems().length > 1'
});
 
ko.applyBindings(new BetterListModel());
```
