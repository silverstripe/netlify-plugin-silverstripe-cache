# Persist the Silverstripe CMS Gatsby Cache Between Builds

Persist the Silverstripe CMS Gatsby cache between Netlify builds to
take advantage of hard cached uploaded files.

For use in conjuction with [netlify-plugin-gatsby-cache](https://github.com/jlengstorf/netlify-plugin-gatsby-cache).

## Usage

You can install this plugin manually using `netlify.toml`. If you want to know more about file-based configuration on Netlify, click [here](https://docs.netlify.com/configure-builds/file-based-configuration/).

Add the following lines to your project's `netlify.toml` file:

```toml
[build]
  publish = "public"

[[plugins]]
  package = "netlify-plugin-gatsby-cache"

[[plugins]]
  package = "netlify-plugin-silverstripe-cache"
```

> Note: The `[[plugins]]` line is required for each plugin, even if you have other plugins in your `netlify.toml` file already.

## Why do I need this?

While the standard `netlify-plugin-gatsby-cache` is great for persisting
data and assets between builds, clearing the cache results in a loss of
all your downloaded files. For many sites, these files can exceed multiple gigabytes, resulting in very long build times on an empty cache.

Because unlikely that once a file is downloaded it would need to be deleted, downloaded assets in the `gatsby-source-silverstripe` plugin
can be given special instructions to be stored _outside the Gatsby cache folder_, making them resillient against standard cache invalidation.

This feature, known as "hard caching" is enabled by default with the `hardCacheAssets` flag in the source plugin. It reconciles the
`.silverstripe-cache` folder with the synced data on every build, meaning that adding, deleting, or updating files will result in a 
parallel update to the hard cache directory.

None of this works, however, if we don't tell Netlify to protect
this folder between builds.

## How does it work?

This plugin determines the location of your `.silverstripe-cache` folder by looking at the `publish` folder configured for Netlify deployment. This means that if your Gatsby site successfully deploys, it will be cached as well with no config required! 🎉


## Want to learn how to create your own Netlify Build Plugins?

Check out [Sarah Drasner’s excellent tutorial](https://www.netlify.com/blog/2019/10/16/creating-and-using-your-first-netlify-build-plugin/?utm_source=github&utm_medium=netlify-plugin-gatsby-cache-jl&utm_campaign=devex)!
