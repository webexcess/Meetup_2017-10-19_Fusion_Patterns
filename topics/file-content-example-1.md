@title[Example 1]
## Hash for ResourceUri
<span class="text-muted">Example 1</span>
```elm
prototype(WebExcess.Theme:HashResourceUri) < prototype(Neos.Fusion:Value) {
	path = null
	resource = null

	@context {
		path = ${this.path}
		resource = ${this.resource}
	}

	fileContent = WebExcess.Theme:FileContent {
		path = ${path}
		resource = ${resource}
	}

	resourceUri = Neos.Fusion:ResourceUri {
		path = ${path}
		resource = ${resource}
	}

	value = ${this.resourceUri + '?h=' + String.md5(this.fileContent)}
}
```
@[10-13]

+++

### How to use it
```elm
prototype(Neos.Neos:Page) {
	head.stylesheets.main = Neos.Fusion:Tag {
		tagName = 'link'
		attributes {
			rel = 'stylesheet'
			href = WebExcess.Theme:HashResourceUri {
				path = 'resource://WebExcess.Theme/Public/Styles/Main.css'
			}
		}
	}
}
```
@[6-8]
