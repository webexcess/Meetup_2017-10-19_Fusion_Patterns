## File content
```elm
prototype(WebExcess.Theme:FileContent) < prototype(Neos.Fusion:Value) {
	@class = 'WebExcess\\Theme\\Fusion\\FileContentImplementation'
	path = null
	resource = null
}
```

+++

### PHP Class
```php
class FileContentImplementation extends AbstractFusionObject
{
    /**
     * The location of the resource, must be a resource://... URI
     *
     * @return string
     */
    public function getPath()
    {
        return $this->fusionValue('path');
    }

    /**
     * If specified, this resource object is used instead of the path and package information
     *
     * @return Resource
     */
    public function getResource()
    {
        return $this->fusionValue('resource');
    }

    /**
     * Returns the file content of a resource. Fails silent
     *
     * @return string | boolean
     */
    public function evaluate()
    {
        $resource = $this->getResource();
        if ($resource) {
            return stream_get_contents($resource->getStream());
        }
        $path = $this->getPath();
        if ($path) {
            return Files::getFileContents($path);
        }
        return false;
    }
}
```
@[23-39]
