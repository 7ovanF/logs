# Composition over Inheritance

A focus on "what it can do" instead of the type.
Exploits "has-a" instead of "is-a".

Inheritance is rigid, as children have a strict contract to the parent. It's just "extension".
Composition, on the other hand, lets you build the initial base object.

You can re-introduce abstraction through interfaces (smaller and more "per-action").

This is an example that CodeAesthetics used to demonstrate.
```java
/**
 * the actual outside interface to images.
 */
class ImageApp {
    Image image; // noun
    FileImage file; // verb
	
    public void loadImage() throws Exception {
        if (file == null) {
            throw new Exception("Can't load this poor guy");
        }
        file.load(image);
    }
	
    public void saveImage() throws Exception {
        if (file == null) {
            throw new Exception("He's beyond saving");
        }
        file.save(image);
	}
}

/**
 * what the data is
 */
class Image {
    // data, metadata, etc here...
    // oh yeah, having a metadata class here would be composition!
    public void show() {
        // show image logic
    }
}

/**
 * abstraction for png, jpg which can load() and save()
 */
interface FileImage{
    public void save(Image image);
    public void load(Image image);
}

/**
 * file types.
 * in the end, these DO rely on Image, but not like it didnt with inheritance. (in fact, more)
 */
class PngImage implements FileImage {
    public void save(Image image) {
        // png logic
    };
    public void load(Image image) {
        // png logic
    };
}
class JpgImage implements FileImage {
    public void save(Image image) {
        // jpg logic
    };
    public void load(Image image) {
        // jpg logic
    };
}

/**
 * the change that rendered initial inheritance unmaintainable
 */
class DrawableImage {
    public void draw(Image image) {
        // draw logic
    }
}

```