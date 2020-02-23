<div align="center">
    <h1>m3u Content Parser</h1>
    <h5>Minimalistic, functional and easy to use playlist parser</h5>
</div>

### Example usage

```php

use zikwall\m3ucontentparser\M3UContentParser;
use zikwall\m3ucontentparser\M3UItem;

$parser = new M3UContentParser('https://iptv-org.github.io/iptv/countries/ru.m3u');
$parser->parse();

foreach ($parser->limit(20)->all() as $item) {
    /**
     * @var $item M3UItem
     */
    echo
        $item->getTvgName(),    PHP_EOL,
        $item->getTvgLogo(),    PHP_EOL,
        $item->getTvgUrl(),     PHP_EOL,
        $item->getGroupTitle(), PHP_EOL,
        $item->getId(),         PHP_EOL;
        $item->getGroupId(),    PHP_EOL,
        $item->getLanguage(),   PHP_EOL,
        $item->getCountry();
}

```

### More example

```php

// rewrite current m3u content
foreach ($parser->all() as $item) {
    /**
     * @var $item M3UItem
     */
     $item
             ->replaceTvgName('UNNAMED ITEM')
             ->replaceTvgLogo('NEW LOGO URL')
             ->replaceTvgUrl('CHANE NEW URL');   
}

echo $parser->rewriteM3UFile();

output example:
#EXTM3U url-tvg="http://iptvx.one/epg/epg.xml.gz"
#EXTINF:-1 group-title="Новости",UNNAMED ITEM
CHANE NEW URL
#EXTINF:-1 group-title="Новости",UNNAMED ITEM
CHANE NEW URL
```

### Segment Parser

```php

use zikwall\m3ucontentparser\SegmentM3U;
$segmant = new SegmentM3U();
$fragment->attachSource('examples/segments/index.m3u8')->parse();

print_r($segment->getFragments());
print_r($segment->getSegments());
print_r($segment->getTime());
var_dump($segment->getIsEnding());

```

### Complex sample

```php

use zikwall\m3ucontentparser\M3UContentParser;
use zikwall\m3ucontentparser\SegmentM3U;
use zikwall\m3ucontentparser\M3UItem;
use zikwall\m3ucontentparser\FragmentM3UItem;

$parser = new M3UContentParser('https://iptv-org.github.io/iptv/countries/ru.m3u');
$parser->parse();

foreach ($parser->limit(10)->offset(10)->all() as $item) {
    /**
     * @var $item M3UItem
     */
    $segment = new SegmentM3U();
    $segment->attachSource($item->getTvgUrl())->parse();

    if ($segment->getIsFragmentSource() === true) {
        /**
         * @var $fragment FragmentM3UItem
         */
        echo $segment->getCleanSource(), PHP_EOL;
        foreach ($segment->getFragments() as $fragment) {
            $subSegment = new SegmentM3U();
            $subSegment->attachSource($segment->getNewSource($fragment->fragment))->parse();

            echo $segment->getNewSource($fragment->fragment), PHP_EOL;
        }

        continue;
    }

    // ... conventional segment processing
    foreach ($segment->getSegments() as $segmentItem) {
        /**
         * @var $segmentItem SegmentM3UItem
         */
        echo $segment->getNewSource($segmentItem->segment), PHP_EOL;
//        display generated chunk links
//        http://xxxxx/stream8/stream2020_2_23_13_44_7-65006.ts
//        http://xxxxx/stream8/stream2020_2_23_13_44_18-65007.ts
//        http://xxxxx/stream8/stream2020_2_23_13_44_28-65008.ts
//        ...
    }
}

```

### More examples Segment parser

You can try different combinations of examples yourself

### API

1. The parser constructor accepts a link or file path:
```php
$parser = new M3UContentParser('https://iptv-org.github.io/iptv/countries/ru.m3u');
```

2. Parsing is called by the same method:
```php
$parser->parse();
```

### Methods in m3u parser

- [x] `getCahce()`
- [x] `getResfresh()`
- [x] `getTvgUrl()`
- [x] `limit(int)`
- [x] `offset(int)`
- [x] `all()`
- [x] `getItems()`
- [x] `rewriteM3UFile()`

### Methods in m3u item object

- [x] `getId()`
- [x] `getTvgId()`
- [x] `getTvgName()`
- [x] `getTvgUrl()`
- [x] `getTvgLogo()`
- [x] `getTvgShift()`
- [x] `getGroupId()`
- [x] `getGroupTitle()`
- [x] `getExtGrp()`
- [x] `getCensored()`
- [x] `getLanguage()`
- [x] `getCountry()`
- [x] `getAudioTrack()`
- [x] `getAudioTrackNum()`
- [x] `getExtraAttributes()`

### API Segment

- [x] `getFragments()` return array of tracks with props
- [x] `getSegments()` return array of segments in track
- [x] `getTime()` return array of timecodes: hours, minutes, seconds and string format HH:MM:SS & original value
- [x] `getDuration()` return float value duration of seconds
- [x] `getIsEnding()` return bool value, indicates whether the stream has an ending or not 

### Installation

`composer require zikwall/m3u-content-parser`

### Develop Mode

```json
{
    "minimum-stability": "dev",
    "repositories": [
        {
            "type": "git",
            "url": "https://github.com/zikwall/m3u-content-parser.git"
        }
    ],
    "require": {
        "zikwall/m3u-content-parser": "dev-master"
    }
}
```

#### Questions?

For all questions and suggestions - welcome to Issues
