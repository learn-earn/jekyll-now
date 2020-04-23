First of all, thanks to Brian from Lets Build That App. I started building complex Views using nested UICollectionView inside UICollectionView. And also it got me thinking, how do I apply the same technics I've used with UITableView to achieve auto cell height on UICollectionView with flowlayout.

I remembered that following his tutorial of recreating Apple App Store app from [https://youtu.be/Ko9oNhlTwH0](https://youtu.be/Ko9oNhlTwH0). I've learn a lot: 
- Programmatically adding UIs
- Adding constraints manually using VFL (Visual Formatting Language) (It was very pain for me)
- Until today I enjoy so much manually adding constrains using Layout Anchors
- Configuring UICollectionViewFlowLayout, calculate it's itemSize - reminds me the old days working with frame
- Nested UICollectionView onto the cell of parent UICollectionView (so many UICollectionView Delegate and DataSource methods to manage, and cell delegate methods to work with actions on the nested UICollectionViews)
- Many more...

It was fun, and so many to learn, but sametime I felt it was a chore, I have repeated myself a lot...

From his inspiration, I also spent a lot of time googling "how to auto cell height for UICollectionViewCell using Auto Layout" (self-sizing challenge), and I watched Apple's video about custom layout over and over again, I've got the essence of it, but I am not talented enough to create my own crazy layout yet. 
finally last year I've discovered Apple's Advances in Collection View Layout from here - [https://developer.apple.com/videos/play/wwdc2019/215/](https://developer.apple.com/videos/play/wwdc2019/215/)

Then I fall in love with the brand-new concrete layout class - [UICollectionViewCompositionalLayout](https://developer.apple.com/documentation/uikit/uicollectionviewcompositionallayout).

It is composable - you can make very complex collection view from simple element.
It is flexible - you can write any layout using it.
And Apple claims it is fast!

![rendered2x-1585241228.png]({{site.baseurl}}/rendered2x-1585241228.png)

What I enjoy using it the most is that it is declaritive. As the image above summaries, you configure three layers/elements to create this layout, starting from inside out, they are item, group, and section.

Let's use one of my project to illustrate how easy to implement the Composational Layout.
My client's requirments are:
- The item Cell has fixed width, but dynamic height based on the subviews/contents it has.
- Displaying a list of items vertically, center the list on screen.
- on larger screen such as iPads, showing two columns.
	- if there is only one item in the list, center the item on screen.
- on smaller screen such as iPhones, showing single column.

The whole code snippt to setup the layout is as below: 
````
let layout = UICollectionViewCompositionalLayout { [weak self] (sectionIndex: Int,
            layoutEnvironment: NSCollectionLayoutEnvironment) -> NSCollectionLayoutSection? in
            
            // Item
            let size = NSCollectionLayoutSize(
                widthDimension: .absolute(articleCellWidth),
                heightDimension: .estimated(articleCellHeight)
            )
            let item = NSCollectionLayoutItem(layoutSize: size)
            item.edgeSpacing = NSCollectionLayoutEdgeSpacing(leading: .none, top: .none, trailing: .none, bottom: .fixed(articleGroupInterItemSpacing))
            
            // Group
            let groupSize = NSCollectionLayoutSize(
                widthDimension: .fractionalWidth(1),
                heightDimension: .estimated(44))
            let group = NSCollectionLayoutGroup.horizontal(layoutSize: groupSize, subitems: [item])
            // Section
            let section = NSCollectionLayoutSection(group: group)
            
            let count = self?.viewModel.itemsToShare.count == 1 ? 1 : 2
            let padding = (layoutEnvironment.container.effectiveContentSize.width - CGFloat(count) * articleCellWidth - articleGroupInterItemSpacing) * 0.5
            
            section.contentInsets = NSDirectionalEdgeInsets(top: 0, leading: padding, bottom: 0, trailing: padding)
            section.interGroupSpacing = 0
            return section
        }
```` 

Combination of using `.estimated(44)` and setting up appropriate Auto Layout constraints enable us to solve the auto-sizing cell challenge.




