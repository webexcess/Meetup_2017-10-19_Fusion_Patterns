## Folder Structure
![Folder Structure](assets/images/folder-structure.jpg)

+++

@title[Prototypes: Root.fusion]
<span class="text-muted">Root.fusion</span>
```elm
root {
	default {
		type = ${q(node).property('_nodeType') + '.Document'}
		renderPath >
	}
}

include: Documents/**/*.fusion
include: NodeTypes/**/*.fusion
include: Prototypes/**/*.fusion
```

+++

@title[Prototypes: Documents]
<span class="text-muted">Documents</span>
```elm
prototype(WebExcess.Theme:Default.Document) < prototype(Page) {
	body = Neos.Fusion:Array {
		header = WebExcess.Theme:Header
		main = WebExcess.Theme:Main
		footer = WebExcess.Theme:Footer
	}
}
```

```elm
prototype(WebExcess.Theme:Page.Document) < prototype(WebExcess.Theme:Default.Document)
```

+++

@title[Prototypes: NodeTypes]
<span class="text-muted">NodeTypes</span>
```elm
prototype(WebExcess.Theme:Button) < prototype(Neos.Fusion:Tag) {
	tagName = 'button'
}
```

+++

@title[Prototypes: Header]
<span class="text-muted">Header</span>
```elm
prototype(WebExcess.Theme:Header) < prototype(Neos.Fusion:Tag) {
	tagName = 'header'
	content = Neos.Fusion:Array {
		logo = WebExcess.Theme:Header.Logo
		navigation = WebExcess.Theme:Header.Navigation
	}
}
```

```elm
prototype(WebExcess.Theme:Header.Logo) < prototype(Neos.Fusion:Tag) {
	tagName = 'img'
}
```

```elm
prototype(WebExcess.Theme:Header.Navigation) < prototype(Neos.Fusion:Tag) {
	tagName = 'nav'
}
```

+++

@title[Prototypes: Main]
<span class="text-muted">Main</span>
```elm
prototype(WebExcess.Theme:Main) < prototype(Neos.Fusion:Tag) {
	tagName = 'main'
	content = WebExcess.Theme:Main.Content
}
```

```elm
prototype(WebExcess.Theme:Main.Content) < prototype(ContentCollection) {
	nodePath = 'main'
}
```

+++

@title[Prototypes: Footer]
<span class="text-muted">Footer</span>
```elm
prototype(WebExcess.Theme:Footer) < prototype(Neos.Fusion:Tag) {
	tagName = 'footer'
	content = Neos.Fusion:Array {
		navigation = WebExcess.Theme:Footer.Navigation
	}
}
```

```elm
prototype(WebExcess.Theme:Footer.Navigation) < prototype(Neos.Fusion:Tag) {
	tagName = 'nav'
}
```

+++

#### Output
```html
<body>
    <header>
        <img ... />
		<nav></nav>
    </header>

    <main>
        <div class="neos-contentcollection"></div>
    </main>

    <footer>
        <nav></nav>
    </footer>

</body>
```
