---
published: true
layout: post
title: Unit test UITableViewCell is selected
excerpt: >-
   This could be a very tiny problem, or something I missed. But I still like to documement it for future reference.
---
This could be a very tiny problem, or something I missed. But I still like to documement it for future reference.

## What is the problem I am solving? (the requirement)
the `TagListsViewController` use `UITableView` to display a list of tags. 
And if the condition is met, we need `UITableView` row to be selected.

### production code as below

```
func tableView(_ tableView: UITableView, willDisplay cell: UITableViewCell, forRowAt indexPath: IndexPath) {
    if condition met {
        tableView.selectRow(at: indexPath, animated: false, scrollPosition: .none)

        // Added this line below for testing
        print("row at \(indexPath.row) isSelected = \(cell.isSelected)")
    }
}
```

### and the testing code as below

```
func test_willDisplay_rendersCellSelectedWhenTagIsAlreadySelected() {
    let sut = makeSUT()
    sut.loadViewIfNeeded()

    sut.simulateCellWillDisplay(at: 0)

    // I started with this Assertion. Let's call it A
    // but it doesn't set isSelected value to true during test
    let viewDisplayed = sut.tagView(at: 0)
    XCTAssertEqual(viewDisplayed?.isSelected, true)
 
    // Assertion which works. Let's call it B
    XCTAssertEqual(sut.tableView.indexPathForSelectedRow, IndexPath(row: 0, section: 0))
}

// Helper methods for tests
func tagView(at row: Int) -> UITableViewCell? {
    let ds = tableView.dataSource
    let index = IndexPath(row: row, section: 0)
    return ds?.tableView(tableView, cellForRowAt: index)
}

func simulateCellWillDisplay(at index: Int) {
    tableView.delegate?.tableView?(tableView, willDisplay: tagView(at index)!, forRowAt: IndexPath(row: index, section: 0))
}
```

## This is the problem I am facing during unit test

When I run unit test, Assertion A will never pass, so in the end I changed it to Assertion B, and it worked.

Also when I run the unit test, this line 
```
print("row at \(indexPath.row) isSelected = \(cell.isSelected)")
```

always get `cell.isSelected ` value as `false`.

But in production, if the condition met, I can get the value as `true`.
