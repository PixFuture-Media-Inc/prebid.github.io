---
layout: page_v2
title: Ad Unit Reference
description: Ad Unit Reference
sidebarType: 1
---

# Ad Unit Reference
{:.no_toc}

The ad unit object is where you configure what kinds of ads you will show in a given ad slot on your page, including:

+ Allowed media types (e.g., banner, native, and/or video)
+ Allowed sizes
+ AdUnit-specific first party data

It's also where you will configure bidders, e.g.:

+ Which bidders are allowed to bid for that ad slot
+ What information is passed to those bidders via their [parameters]({{site.baseurl}}/dev-docs/bidders.html)

This page describes the properties of the `adUnit` object.

* TOC
{:toc}

## AdUnit

See the table below for the list of properties on the ad unit.  For example ad units, see the [Examples](#adUnit-examples) below.

{: .table .table-bordered .table-striped }
| Name         | Scope    | Type                                  | Description                                                                                                                                                                                |
|--------------+----------+---------------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `code`       | Required | String                                | An identifier you create and assign to this ad unit. Generally this is set to the ad slot name or the div element ID. Used by [setTargetingForGPTAsync()](/dev-docs/publisher-api-reference/setTargetingForGPTAsync.html) to match which auction is for which ad slot. |
| `sizes`      | Required | Array[Number] or Array[Array[Number]] | All sizes this ad unit can accept.  Examples: `[400, 600]`, `[[300, 250], [300, 600]]`.  For 1.0 and later, define sizes within the appropriate `mediaTypes.{banner,native,video}` object. |
| `bids`       | Required | Array[Object]                         | Array of bid objects representing demand partners and associated parameters for a given ad unit.  See [Bids](#adUnit.bids) below.                                                          |
| `mediaTypes` | Optional | Object                                | Defines one or more media types that can serve into the ad unit.  For a list of properties, see [`adUnit.mediaTypes`](#adUnit.mediaTypes) below.                                           |
| `labelAny`   | Optional | Array[String]                         | Used for [conditional ads][conditionalAds].  Works with `sizeConfig` argument to [pbjs.setConfig][configureResponsive].                                                                    |
| `labelAll`   | Optional | Array[String]                         | Used for [conditional ads][conditionalAds]. Works with `sizeConfig` argument to [pbjs.setConfig][configureResponsive].                                                                     |
| `ortb2Imp`   | Optional | Object                         | ortb2Imp is used to signal OpenRTB Imp objects at the adUnit grain. Similar to the global ortb2 field used for [global first party data configuration](/dev-docs/publisher-api-reference/setConfig.html#setConfig-fpd), but specific to this adunit. The ortb2Imp object currently supports [first party data](#adUnit-fpd-example) including the [Prebid Ad Slot](/features/pbAdSlot.html) and the [insterstitial](#adUnit-interstitial-example) signal. |

<a name="adUnit.bids" />

### adUnit.bids

See the table below for the list of properties in the `bids` array of the ad unit.  For example ad units, see the [Examples](#adUnit-examples) below.

{: .table .table-bordered .table-striped }
| Name       | Scope    | Type          | Description                                                                                                                              |
|------------+----------+---------------+------------------------------------------------------------------------------------------------------------------------------------------|
| `bidder`   | Required | String        | Unique code identifying the bidder. For bidder codes, see the [bidder param reference]({{site.baseurl}}/dev-docs/bidders.html).          |
| `params`   | Required | Object        | Bid request parameters for a given bidder. For allowed params, see the [bidder param reference]({{site.baseurl}}/dev-docs/bidders.html). |
| `labelAny` | Optional | Array[String] | Used for [conditional ads][conditionalAds].  Works with `sizeConfig` argument to [pbjs.setConfig][configureResponsive].                  |
| `labelAll` | Optional | Array[String] | Used for [conditional ads][conditionalAds]. Works with `sizeConfig` argument to [pbjs.setConfig][configureResponsive].                   |

<a name="adUnit.mediaTypes" />

### adUnit.mediaTypes

See the table below for the list of properties in the `mediaTypes` object of the ad unit.  For example ad units showing the different media types, see the [Examples](#adUnit-examples) below.

{: .table .table-bordered .table-striped }
| Name                                  | Scope                                                                    | Type   | Description                                                                                                      |
|---------------------------------------+--------------------------------------------------------------------------+--------+------------------------------------------------------------------------------------------------------------------|
| [`banner`](#adUnit.mediaTypes.banner) | At least one of the `banner`, `native`, or `video` objects are required. | Object | Defines properties of a banner ad.  For examples, see [`adUnit.mediaTypes.banner`](#adUnit.mediaTypes.banner).   |
| [`native`](#adUnit.mediaTypes.native) | At least one of the `banner`, `native`, or `video` objects are required. | Object | Defines properties of a native ad.  For properties, see [`adUnit.mediaTypes.native`](#adUnit.mediaTypes.native). |
| [`video`](#adUnit.mediaTypes.video)   | At least one of the `banner`, `native`, or `video` objects are required. | Object | Defines properties of a video ad.  For examples, see [`adUnit.mediaTypes.video`](#adUnit.mediaTypes.video).      |

<a name="adUnit.mediaTypes.banner" />

#### adUnit.mediaTypes.banner

{: .table .table-bordered .table-striped }
| Name    | Scope    | Type                                  | Description                                                                             |
|---------+----------+---------------------------------------+-----------------------------------------------------------------------------------------|
| `sizes` | Required | Array[Number] or Array[Array[Number]] | All sizes this ad unit can accept.  Examples: `[400, 600]`, `[[300, 250], [300, 600]]`. |
| `pos`  | Optional | Integer                                | OpenRTB page position value: 0=unknown, 1=above-the-fold, 3=below-the-fold, 4=header, 5=footer, 6=sidebar, 7=full-screen   |
| `name`  | Optional | String                                | Name for this banner ad unit.  Can be used for testing and debugging.                   |

<a name="adUnit.mediaTypes.native" />

#### adUnit.mediaTypes.native

The `native` object contains the following properties that correspond to the assets of the native ad.

{: .table .table-bordered .table-striped }
| Name          | Scope    | Type   | Description                                                                                                                                                                                                          |
|---------------+----------+--------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `type`        | Optional | String | A [pre-defined native type]({{site.baseurl}}/dev-docs/show-native-ads.html#pre-defined-native-types) used as a shorthand, e.g., `type: 'image'` implies required fields `image`, `title`, `sponsoredBy`, `clickUrl`. |
| `title`       | Optional | Object | The title object is to be used for the title element of the native ad.  For properties, see [`native.title`](#adUnit.mediaTypes.native.title).                                                                       |
| `body`        | Optional | Object | The body object is to be used for the body element of the native ad.  For properties, see [`native.body`](#adUnit.mediaTypes.native.body).                                                                           |
| `sponsoredBy` | Optional | Object | The name of the brand associated with the ad.  For properties, see [`native.sponsoredBy`](#adUnit.mediaTypes.native.sponsoredby).                                                                                    |
| `icon`        | Optional | Object | The brand icon that will appear with the ad.  For properties, see [`native.icon`](#adUnit.mediaTypes.native.icon).                                                                                                   |
| `image`       | Optional | Object | The image object is to be used for the main image of the native ad.  For properties, see [`native.image`](#adUnit.mediaTypes.native.image).                                                                          |
| `clickUrl`    | Optional | Object | Where the user will end up if they click the ad.  For properties, see [`native.clickUrl`](#adUnit.mediaTypes.native.clickUrl).                                                                                       |
| `cta`         | Optional | String | *Call to Action* text, e.g., "Click here for more information".  |

<a name="adUnit.mediaTypes.native.image" />

##### adUnit.mediaTypes.native.image

{: .table .table-bordered .table-striped }
| Name            | Scope    | Type                                  | Description                                                                                                                                           |
|-----------------+----------+---------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------|
| `required`      | Optional | Boolean                               | Whether this asset is required.                                                                                                                       |
| `sizes`         | Optional | Array[Number] or Array[Array[Number]] | All sizes this ad unit can accept.  Examples: `[400, 600]`, `[[300, 250], [300, 600]]`.                                                               |
| `aspect_ratios` | Optional | Array[Object]                         | Alongside `sizes`, you can define allowed aspect ratios.  For properties, see [`image.aspect_ratios`](#adUnit.mediaTypes.native.image.aspect_ratios). |

<a name="adUnit.mediaTypes.native.image.aspect_ratios" />

###### adUnit.mediaTypes.native.image.aspect_ratios

{: .table .table-bordered .table-striped }
| Name           | Scope    | Type    | Description                                                                                          |
|----------------+----------+---------+------------------------------------------------------------------------------------------------------|
| `min_height`   | Optional | Integer | The minimum height required for an image to serve (in pixels).                                       |
| `min_width`    | Optional | Integer | The minimum width required for an image to serve (in pixels).                                        |
| `ratio_height` | Required | Integer | This, combined with `ratio_width`, determines the required aspect ratio for an image that can serve. |
| `ratio_width`  | Required | Integer | See above.                                                                                           |

<a name="adUnit.mediaTypes.native.title" />

##### adUnit.mediaTypes.native.title

{: .table .table-bordered .table-striped }
| Name       | Scope    | Type    | Description                                          |
|------------+----------+---------+------------------------------------------------------|
| `required` | Optional | Boolean | Whether a title asset is required on this native ad. |
| `len`      | Optional | Integer | Maximum length of title text, in characters.         |

<a name="adUnit.mediaTypes.native.sponsoredBy" />

##### adUnit.mediaTypes.native.sponsoredBy

{: .table .table-bordered .table-striped }
| Name       | Scope    | Type    | Description                                               |
|------------+----------+---------+-----------------------------------------------------------|
| `required` | Optional | Boolean | Whether a brand name asset is required on this native ad. |

<a name="adUnit.mediaTypes.native.clickUrl" />

##### adUnit.mediaTypes.native.clickUrl

{: .table .table-bordered .table-striped }
| Name       | Scope    | Type    | Description                                              |
|------------+----------+---------+----------------------------------------------------------|
| `required` | Optional | Boolean | Whether a click URL asset is required on this native ad. |

<a name="adUnit.mediaTypes.native.body" />

##### adUnit.mediaTypes.native.body

{: .table .table-bordered .table-striped }
| Name       | Scope    | Type    | Description                                       |
|------------+----------+---------+---------------------------------------------------|
| `required` | Optional | Boolean | Whether body text is required for this native ad. |
| `len`      | Optional | Integer | Maximum length of body text, in characters.       |

<a name="adUnit.mediaTypes.native.icon" />

##### adUnit.mediaTypes.native.icon

{: .table .table-bordered .table-striped }
| Name            | Scope    | Type                                  | Description                                                                                                                                          |
|-----------------+----------+---------------------------------------+------------------------------------------------------------------------------------------------------------------------------------------------------|
| `required`      | Optional | Boolean                               | Whether an icon asset is required on this ad.                                                                                                        |
| `sizes`         | Optional | Array[Number] or Array[Array[Number]] | All sizes this ad unit can accept.  Examples: `[400, 600]`, `[[300, 250], [300, 600]]`.                                                              |
| `aspect_ratios` | Optional | Array[Object]                         | Instead of `sizes`, you can define allowed aspect ratios.  For properties, see [`icon.aspect_ratios`](#adUnit.mediaTypes.native.icon.aspect_ratios). |

<a name="adUnit.mediaTypes.native.icon.aspect_ratios" />

###### adUnit.mediaTypes.native.icon.aspect_ratios

{: .table .table-bordered .table-striped }
| Name           | Scope    | Type    | Description                                                                                          |
|----------------+----------+---------+------------------------------------------------------------------------------------------------------|
| `min_width`    | Optional | Integer | The minimum width required for an image to serve (in pixels).                                        |
| `ratio_height` | Required | Integer | This, combined with `ratio_width`, determines the required aspect ratio for an image that can serve. |
| `ratio_width`  | Required | Integer | See above.                                                                                           |

<a name="adUnit.mediaTypes.video" />

#### adUnit.mediaTypes.video

{: .table .table-bordered .table-striped }
| Name             | Scope       | Type                   | Description                                                                                                                                                         |
|------------------+-------------+------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `pos`  | Optional | Integer                                | OpenRTB page position value: 0=unknown, 1=above-the-fold, 3=below-the-fold, 4=header, 5=footer, 6=sidebar, 7=full-screen   |
| `context`        | Recommended    | String                 | The video context, either `'instream'`, `'outstream'`, or `'adpod'` (for long-form videos).  Example: `context: 'outstream'`. Defaults to 'instream'. |
| `placement`        | Recommended    | Integer                 | 1=in-stream, 2=in-banner, 3=in-article, 4=in-feed, 5=interstitial/floating. **Highly recommended** because some bidders require more than context=outstream. |
| `playerSize`     | Optional    | Array[Integer,Integer] | The size (width, height) of the video player on the page, in pixels.  Example: `playerSize: [640, 480]`                                                                                                  |
| `api`            | Recommended | Array[Integer]         | List of supported API frameworks for this impression.  If an API is not explicitly listed, it is assumed not to be supported.  For list, see [OpenRTB spec][openRTB].                                                  |
| `mimes`          | Recommended | Array[String]          | Content MIME types supported, e.g., `"video/x-ms-wmv"`, `"video/mp4"`. **Required by OpenRTB when using [Prebid Server][pbServer]**.                                                                                   |
| `protocols`      | Optional    | Array[Integer]         | Array of supported video protocols.  For list, see [OpenRTB spec][openRTB]. **Required by OpenRTB when using [Prebid Server][pbServer]**.                                                                            |
| `playbackmethod` | Optional    | Array[Integer]         | Allowed playback methods. If none specified, all are allowed.  For list, see [OpenRTB spec][openRTB]. **Required by OpenRTB when using [Prebid Server][pbServer]**.                                                     |
| `minduration`      | Recommended    | Integer         | Minimum video ad duration in seconds, see [OpenRTB spec][openRTB].           |
| `maxduration`      | Recommended    | Integer         | Maximum video ad duration in seconds, see [OpenRTB spec][openRTB].           |
| `w`      | Recommended    | Integer         |
Width of the video player in device independent pixels (DIPS)., see [OpenRTB spec][openRTB].           |
| `h`      | Recommended    | Integer         | Height of the video player in device independent pixels (DIPS)., see [OpenRTB spec][openRTB].           |
| `startdelay`      | Recommended    | Integer         | Indicates the start delay in seconds, see [OpenRTB spec][openRTB].           |
| `placement`      | Optional    | Integer         | Placement type for the impression, see [OpenRTB spec][openRTB].           |
| `linearity`      | Optional    | Integer         | Indicates if the impression must be linear, nonlinear, etc, see [OpenRTB spec][openRTB].           |
| `skip`      | Optional    | Integer         | Indicates if the player will allow the video to be skipped,
where 0 = no, 1 = yes., see [OpenRTB spec][openRTB].           |
| `skipmin`      | Optional    | Integer         | Videos of total duration greater than this number of seconds
can be skippable; only applicable if the ad is skippable., see [OpenRTB spec][openRTB].           |
| `skipafter`      | Optional    | Integer         | Number of seconds a video must play before skipping is
enabled; only applicable if the ad is skippable., see [OpenRTB spec][openRTB].           |
| `minbitrate`      | Optional    | Integer         | Minimum bit rate in Kbps., see [OpenRTB spec][openRTB].           |
| `maxbitrate`      | Optional    | Integer         | Maximum bit rate in Kbps., see [OpenRTB spec][openRTB].           |
| `delivery`      | Optional    | Array[Integer]         | Supported delivery methods (e.g., streaming, progressive), see [OpenRTB spec][openRTB].           |
| `pos`      | Optional    | Integer         | Ad position on screen, see [OpenRTB spec][openRTB].           |
| `playbackend`      | Optional    | Integer         | The event that causes playback to end, see [OpenRTB spec][openRTB].           |

If `'video.context'` is set to `'adpod'` then the following parameters are also available.  

{: .table .table-bordered .table-striped }
| Name             | Scope       | Type                   | Description                                                                                                                                                         |
|------------------+-------------+------------------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `adPodDurationSec`        | Required    | Number                 | The length of the adpod in seconds. Example: `adPodDurationSec = 120` |
| `durationRangeSec`        | Required    | Array[Number]                 | An array of  numbers represents a list of the potential/accepted duration values that the creatives can be in the adpod block. Example: `durationRangeSec = [30, 60, 90]` |
| `requireExactDuration`        | Optional    | Boolean                 | Whether the returned creatives running time must match the value of `adPodDurationSec`. Example: `requireExactDuration = true` |
| `tvSeriesName`        | Optional    | String                 | The name of the television series video the adpod will appear in. Example: `tvSeriesName = 'Once Upon A Time'` |
| `tvEpisodeName`        | Optional    | String                 | The name of the episode of the television series video the adpod will appear in. Example: `tvEpisodeName = 'Pilot'` |
| `tvSeasonNumber`        | Optional    | Number                 | A number representing the season number of the television series video  the adpod will appear in. Example: `tvSeasonNumber = 1` |
| `tvEpisodeNumber`        | Optional    | Number                 | A number representing the episode number of the television series video the adpod will appear in. Example: `tvEpisodeNumber = 1` |
| `contentLengthSec`        | Optional    | Number                 | A number representing the length of the video in seconds. Example: `contentLengthSec = 1` |
| `contentMode`        | Optional    | String                 | A string indicating the type of content being displayed in the video player. There are two options, `live` and `on-demand`. Example: `contentMode = 'on-demand'` |


<a name="adUnit-examples" />

## Examples

+ [Banner](#adUnit-banner-example)
+ [Video](#adUnit-video-example)  
  - [Instream](#adUnit-video-example-instream)  
  - [Outstream](#adUnit-video-example-outstream)  
  - [Adpod (Long-Form)](#adUnit-video-example-adpod)
+ [Native](#adUnit-native-example)
+ [Multi-Format](#adUnit-multi-format-example)
+ [Twin Codes](#adUnit-twin-codes-example)
+ [First Party Data](#adUnit-fpd-example)

<a name="adUnit-banner-example">

### Banner

For an example of a banner ad unit, see below.  For more detailed instructions, see [Getting Started]({{site.baseurl}}/dev-docs/getting-started.html).

```javascript
pbjs.addAdUnits({
    code: slot.code,
    mediaTypes: {
        banner: {
            sizes: [[300, 250]]
        }
    },
    bids: [
        {
            bidder: 'appnexus',
            params: {
                placementId: 13144370
            }
        }
    ]
});
```

<a name="adUnit-video-example">

### Video

<a name="adUnit-video-example-instream">

#### Instream

For an example of an instream video ad unit, see below.  For more detailed instructions, see [Show Video Ads]({{site.baseurl}}/dev-docs/show-video-with-a-dfp-video-tag.html).

```javascript
pbjs.addAdUnits({
    code: slot.code,
    mediaTypes: {
        video: {
            context: 'instream',
            playerSize: [640, 480],
	    w: 640,
	    h: 480,
	    skip: 1,
	    playbackmethod: [2]
        },
    },
    bids: [{
        bidder: 'appnexus',
        params: {
            placementId: 13232361
        }
    }]
});
```

<a name="adUnit-video-example-outstream">

#### Outstream

For an example of an outstream video ad unit, see below.  For more detailed instructions, see [Show Outstream Video Ads]({{site.baseurl}}/dev-docs/show-outstream-video-ads.html).

```javascript
pbjs.addAdUnits({
    code: slot.code,
    mediaTypes: {
        video: {
            context: 'outstream',
            playerSize: [640, 480]
        }
    },
    renderer: {
        url: 'https://cdn.adnxs.com/renderer/video/ANOutstreamVideo.js',
        render: function(bid) {
            ANOutstreamVideo.renderAd({
                targetId: bid.adUnitCode,
                adResponse: bid.adResponse,
            });
        }
    },
    ...
});
```
<a name="adUnit-video-example-adpod">

#### Adpod (Long-Form)

For an example of an adpod video ad unit, see below.  For more detailed instructions, see [Show Long-Form Video Ads]({{site.baseurl}}/prebid-video/video-long-form.html).

```javascript
var longFormatAdUnit = {
    video: {
       // required params
       context: 'adpod',
       playerSize: [640, 480],
       adPodDurationSec: 300,
       durationRangeSec: [15, 30],

       // optional params
       requireExactDuration: true,
       tvSeriesName: 'TvName',
       tvEpisodeName: 'episodeName',
       tvSeasonNumber: 3,
       tvEpisodeNumber: 6,
       contentLength: 300, // time in seconds,
       contentMode: 'on-demand'
    }

    bids: [{
            bidder: 'appnexus',
            params: {
                placementId: '123456789',
            }
        }]
}
```

<a name="adUnit-native-example">

### Native

For an example of a native ad unit, see below.  For more detailed instructions, see [Show Native Ads]({{site.baseurl}}/dev-docs/show-native-ads.html).

```javascript
pbjs.addAdUnits({
    code: slot.code,
    mediaTypes: {
        native: {
            image: {
                required: true,
                sizes: [150, 50]
            },
            title: {
                required: true,
                len: 80
            },
            sponsoredBy: {
                required: true
            },
            clickUrl: {
                required: true
            },
            body: {
                required: true
            },
            icon: {
                required: true,
                sizes: [50, 50]
            }
        }
    },
    bids: [
        {
            bidder: 'appnexus',
            params: {
                placementId: 13232354
            }
        }
    ]
});
```

<a name="adUnit-multi-format-example">

### Multi-Format

For an example of a multi-format ad unit, see below.  For more detailed instructions, see [Show Multi-Format Ads]({{site.baseurl}}/dev-docs/show-multi-format-ads.html).

{% highlight js %}

pbjs.addAdUnits([{
        code: 'div-banner-native',
        mediaTypes: {
            banner: {
                sizes: [
                    [300, 250]
                ]
            },
            native: {
                type: 'image'
            },
        },
        bids: [{
            bidder: 'appnexus',
            params: {
                placementId: 13232392,
            }
        }]
    },

    {
        code: 'div-banner-outstream',
        mediaTypes: {
            banner: {
                sizes: [
                    [300, 250]
                ]
            },
            video: {
                context: 'outstream',
                playerSize: [300, 250]
            },
        },
        bids: [{
            bidder: 'appnexus',
            params: {
                placementId: 13232392,
            }
        }, ]
    },

    {
        code: 'div-banner-outstream-native',
        mediaTypes: {
            banner: {
                sizes: [
                    [300, 250]
                ]
            },
            native: {
                type: 'image'
            },
            video: {
                context: 'outstream',
                playerSize: [300, 250]
            },
        },
        bids: [{
            bidder: 'appnexus',
            params: {
                placementId: 13232392,
            }
        }, ]
    }
]);

{% endhighlight %}

<a name="adUnit-twin-codes-example">

### Twin AdUnit Codes

It's ok to have multiple AdUnits with the same `code`. This can be useful in scenarios
where bidders have different capabilities for the same spot on the page. e.g.

- BidderA should receive both media types, while BidderB gets only one
- BidderA gets one size while BidderB gets another

In this example, bidderA gets both banner and outstream, while bidderB gets only banner.
{% highlight js %}
    var adUnits = [
           {
               code: 'test-div',
                mediaTypes: {
                    video: {
                        context: "outstream",
                        playerSize: [[300,250]]
                    }
                },
               bids: [
                   {
                        bidder: 'bidderA',
                        params: {
                             ...
                        }
                   }
               ]
           },
           {
               code: 'test-div',
                mediaTypes: {
                    banner: {
                        sizes: [[300,250],[300,600],[728,90]]
                    }
                },
               bids: [
                   {
                       bidder: 'bidderB',
                       params: {
                           ...
                       }
                   },{
                        bidder: 'bidderA',
                        params: {
                           ...
                        }
                   }
               ]
           }
       ];
{% endhighlight %}

In this example, bidderA receives 2 bidRequest objects while bidderB receives one. If a bidder provides more than one bid for the same AdUnit.code, Prebid.js will use the highest bid when it's
time to set targeting.

<a name="adUnit-fpd-example">

### First Party Data

Example of an adunit-specific block of first party data:

{% highlight js %}
pbjs.addAdUnits({
    code: "test-div",
    mediaTypes: {
        banner: {
            sizes: [[300,250]]
        }
    },
    ortb2Imp: {
         ext: {
	    data: {
                pbadslot: "homepage-top-rect",
                adUnitSpecificContextAttribute: "123"
	    }
         }
    },
    ...
});
{% endhighlight %}

Notes:
- Only contextual data should be added on the AdUnit; user-related data goes in the [global first party data](/dev-docs/publisher-api-reference/setConfig.html#setConfig-fpd) config.
- For additional help with analytics and reporting you can use the [Prebid Ad Slot](/features/pbAdSlot.html), a special type of first party data.

<a name="adUnit-interstitial-example">

### Interstitial Ads

Example of an adunit-specific interstitial signal:

{% highlight js %}
pbjs.addAdUnits({
    code: "test-div",
    mediaTypes: {
        banner: {
            sizes: [[300,250]]
        }
    },
    ortb2Imp: {
    	instl:1
    },
    ...
});
{% endhighlight %}

For more information on Interstitial ads, reference the [Interstitial feature page](/features/InterstitialAds.html).

## Related Topics

+ [Publisher API Reference]({{site.baseurl}}/dev-docs/publisher-api-reference/)
+ [Conditional Ad Units][conditionalAds]
+ [Show Native Ads]({{site.baseurl}}/dev-docs/show-native-ads.html)
+ [Show Video Ads]({{site.baseurl}}/dev-docs/show-video-with-a-dfp-video-tag.html)
+ [Show Outstream Video Ads]({{site.baseurl}}/dev-docs/show-outstream-video-ads.html)
+ [Show Long-Form Video Ads]({{site.baseurl}}/prebid-video/video-long-form.html)
+ [Prebid.org Video Examples]({{site.baseurl}}/examples/video/)
+ [Prebid.org Native Examples](/dev-docs//examples/native-ad-example.html)



<!-- Reference Links -->

[conditionalAds]: {{site.baseurl}}/dev-docs/conditional-ad-units.html
[setConfig]: {{site.baseurl}}/dev-docs/publisher-api-reference/setConfig.html
[configureResponsive]: {{site.baseurl}}/dev-docs/publisher-api-reference/setConfig.html#setConfig-Configure-Responsive-Ads
[openRTB]: https://www.iab.com/wp-content/uploads/2016/03/OpenRTB-API-Specification-Version-2-5-FINAL.pdf
[pbServer]: {{site.baseurl}}/prebid-server/overview/prebid-server-overview.html
