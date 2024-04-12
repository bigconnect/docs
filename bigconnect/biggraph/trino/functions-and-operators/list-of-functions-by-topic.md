# List of functions by topic

Aggregate#

For more details, see Aggregate functions

·        any\_value()

·        approx\_distinct()

·        approx\_most\_frequent()

·        approx\_percentile()

·        approx\_set()

·        arbitrary()

·        array\_agg()

·        avg()

·        bitwise\_and\_agg()

·        bitwise\_or\_agg()

·        bool\_and()

·        bool\_or()

·        checksum()

·        corr()

·        count()

·        count\_if()

·        covar\_pop()

·        covar\_samp()

·        every()

·        geometric\_mean()

·        histogram()

·        kurtosis()

·        map\_agg()

·        map\_union()

·        max()

·        max\_by()

·        merge()

·        min()

·        min\_by()

·        multimap\_agg()

·        numeric\_histogram()

·        qdigest\_agg()

·        regr\_intercept()

·        regr\_slope()

·        skewness()

·        sum()

·        stddev()

·        stddev\_pop()

·        stddev\_samp()

·        tdigest\_agg()

·        variance()

·        var\_pop()

·        var\_samp()

Array#

For more details, see Array functions and operators

·        all\_match()

·        any\_match()

·        array\_distinct()

·        array\_except()

·        array\_intersect()

·        array\_join()

·        array\_max()

·        array\_min()

·        array\_position()

·        array\_remove()

·        array\_sort()

·        array\_union()

·        arrays\_overlap()

·        cardinality()

·        combinations()

·        concat()

·        contains()

·        element\_at()

·        filter()

·        flatten()

·        ngrams()

·        none\_match()

·        reduce()

·        repeat()

·        reverse()

·        sequence()

·        shuffle()

·        slice()

·        transform()

·        trim\_array()

·        zip()

·        zip\_with()

Binary#

For more details, see Binary functions and operators

·        concat()

·        crc32()

·        from\_base32()

·        from\_base64()

·        from\_base64url()

·        from\_big\_endian\_32()

·        from\_big\_endian\_64()

·        from\_hex()

·        from\_ieee754\_32()

·        from\_ieee754\_64()

·        hmac\_md5()

·        hmac\_sha1()

·        hmac\_sha256()

·        hmac\_sha512()

·        length()

·        lpad()

·        md5()

·        murmur3()

·        reverse()

·        rpad()

·        sha1()

·        sha256()

·        sha512()

·        spooky\_hash\_v2\_32()

·        spooky\_hash\_v2\_64()

·        substr()

·        to\_base32()

·        to\_base64()

·        to\_base64url()

·        to\_big\_endian\_32()

·        to\_big\_endian\_64()

·        to\_hex()

·        to\_ieee754\_32()

·        to\_ieee754\_64()

·        xxhash64()

Bitwise#

For more details, see Bitwise functions

·        bit\_count()

·        bitwise\_and()

·        bitwise\_left\_shift()

·        bitwise\_not()

·        bitwise\_or()

·        bitwise\_right\_shift()

·        bitwise\_right\_shift\_arithmetic()

·        bitwise\_xor()

Color#

For more details, see Color functions

·        bar()

·        color()

·        render()

·        rgb()

Comparison#

For more details, see Comparison functions and operators

·        greatest()

·        least()

Conditional#

For more details, see Conditional expressions

·        case

·        coalesce

·        if

·        nullif

·        try

Conversion#

For more details, see Conversion functions

·        cast()

·        format()

·        try\_cast()

·        typeof()

Date and time#

For more details, see Date and time functions and operators

·        AT TIME ZONE

·        current\_date

·        current\_time

·        current\_timestamp

·        localtime

·        localtimestamp

·        current\_timezone()

·        date()

·        date\_add()

·        date\_diff()

·        date\_format()

·        date\_parse()

·        date\_trunc()

·        format\_datetime()

·        from\_iso8601\_date()

·        from\_iso8601\_timestamp()

·        from\_unixtime()

·        from\_unixtime\_nanos()

·        human\_readable\_seconds()

·        last\_day\_of\_month()

·        now()

·        parse\_duration()

·        to\_iso8601()

·        to\_milliseconds()

·        to\_unixtime()

·        with\_timezone()

Geospatial#

For more details, see Geospatial functions

·        bing\_tile()

·        bing\_tile\_at()

·        bing\_tile\_coordinates()

·        bing\_tile\_polygon()

·        bing\_tile\_quadkey()

·        bing\_tile\_zoom\_level()

·        bing\_tiles\_around()

·        convex\_hull\_agg()

·        from\_encoded\_polyline()

·        from\_geojson\_geometry()

·        geometry\_from\_hadoop\_shape()

·        geometry\_invalid\_reason()

·        geometry\_nearest\_points()

·        geometry\_to\_bing\_tiles()

·        geometry\_union()

·        geometry\_union\_agg()

·        great\_circle\_distance()

·        line\_interpolate\_point()

·        line\_locate\_point()

·        simplify\_geometry()

·        ST\_Area()

·        ST\_AsBinary()

·        ST\_AsText()

·        ST\_Boundary()

·        ST\_Buffer()

·        ST\_Centroid()

·        ST\_Contains()

·        ST\_ConvexHull()

·        ST\_CoordDim()

·        ST\_Crosses()

·        ST\_Difference()

·        ST\_Dimension()

·        ST\_Disjoint()

·        ST\_Distance()

·        ST\_EndPoint()

·        ST\_Envelope()

·        ST\_Equals()

·        ST\_ExteriorRing()

·        ST\_Geometries()

·        ST\_GeometryFromText()

·        ST\_GeometryN()

·        ST\_GeometryType()

·        ST\_GeomFromBinary()

·        ST\_InteriorRings()

·        ST\_InteriorRingN()

·        ST\_Intersects()

·        ST\_Intersection()

·        ST\_IsClosed()

·        ST\_IsEmpty()

·        ST\_IsSimple()

·        ST\_IsRing()

·        ST\_IsValid()

·        ST\_Length()

·        ST\_LineFromText()

·        ST\_LineString()

·        ST\_MultiPoint()

·        ST\_NumGeometries()

·        ST\_NumInteriorRing()

·        ST\_NumPoints()

·        ST\_Overlaps()

·        ST\_Point()

·        ST\_PointN()

·        ST\_Points()

·        ST\_Polygon()

·        ST\_Relate()

·        ST\_StartPoint()

·        ST\_SymDifference()

·        ST\_Touches()

·        ST\_Union()

·        ST\_Within()

·        ST\_X()

·        ST\_XMax()

·        ST\_XMin()

·        ST\_Y()

·        ST\_YMax()

·        ST\_YMin()

·        to\_encoded\_polyline()

·        to\_geojson\_geometry()

·        to\_geometry()

·        to\_spherical\_geography()

HyperLogLog#

For more details, see HyperLogLog functions

·        approx\_set()

·        cardinality()

·        empty\_approx\_set()

·        merge()

JSON#

For more details, see JSON functions and operators

·        is\_json\_scalar()

·        json\_array()

·        json\_array\_contains()

·        json\_array\_get()

·        json\_array\_length()

·        json\_exists()

·        json\_extract()

·        json\_extract\_scalar()

·        json\_format()

·        json\_parse()

·        json\_object()

·        json\_query()

·        json\_size()

·        json\_value()

Lambda#

For more details, see Lambda expressions

·        any\_match()

·        reduce\_agg()

·        regexp\_replace()

·        transform()

Machine learning#

For more details, see Machine learning functions

·        classify()

·        features()

·        learn\_classifier()

·        learn\_libsvm\_classifier()

·        learn\_libsvm\_regressor()

·        learn\_regressor()

·        regress()

Map#

For more details, see Map functions and operators

·        cardinality()

·        element\_at()

·        map()

·        map\_concat()

·        map\_entries()

·        map\_filter()

·        map\_from\_entries()

·        map\_keys()

·        map\_values()

·        map\_zip\_with()

·        multimap\_from\_entries()

·        transform\_keys()

·        transform\_values()

Math#

For more details, see Mathematical functions and operators

·        abs()

·        acos()

·        asin()

·        atan()

·        beta\_cdf()

·        cbrt()

·        ceil()

·        cos()

·        cosh()

·        cosine\_similarity()

·        degrees()

·        e()

·        exp()

·        floor()

·        from\_base()

·        infinity()

·        inverse\_beta\_cdf()

·        inverse\_normal\_cdf()

·        is\_finite()

·        is\_nan()

·        ln()

·        log()

·        log2()

·        log10()

·        mod()

·        nan()

·        normal\_cdf()

·        pi()

·        pow()

·        power()

·        radians()

·        rand()

·        random()

·        round()

·        sign()

·        sin()

·        sinh()

·        sqrt()

·        tan()

·        tanh()

·        to\_base()

·        truncate()

·        width\_bucket()

·        wilson\_interval\_lower()

·        wilson\_interval\_upper()

Quantile digest#

For more details, see Quantile digest functions

·        merge()

·        qdigest\_agg()

·        value\_at\_quantile()

·        values\_at\_quantiles()

Regular expression#

For more details, see Regular expression functions

·        regexp\_count()

·        regexp\_extract()

·        regexp\_extract\_all()

·        regexp\_like()

·        regexp\_position()

·        regexp\_replace()

·        regexp\_split()

Row pattern recognition expressions#

·        classifier

·        first

·        last

·        match\_number

·        next

·        permute

·        prev

Session#

For more details, see Session information

·        current\_catalog

·        current\_groups()

·        current\_schema

·        current\_user

Set Digest#

For more details, see Set Digest functions

·        make\_set\_digest()

·        merge\_set\_digest()

·        cardinality()

·        intersection\_cardinality()

·        jaccard\_index()

·        hash\_counts()

String#

For more details, see String functions and operators

·        chr()

·        codepoint()

·        concat()

·        concat\_ws()

·        format()

·        from\_utf8()

·        hamming\_distance()

·        length()

·        levenshtein\_distance()

·        lower()

·        lpad()

·        ltrim()

·        luhn\_check()

·        normalize()

·        position()

·        replace()

·        reverse()

·        rpad()

·        rtrim()

·        soundex()

·        split()

·        split\_part()

·        split\_to\_map()

·        split\_to\_multimap()

·        starts\_with()

·        strpos()

·        substr()

·        substring()

·        to\_utf8()

·        translate()

·        trim()

·        upper()

·        word\_stem()

System#

For more details, see System information

·        version()

Table#

For more details, see Table functions

·        exclude\_columns()

·        sequence()

T-Digest#

For more details, see T-Digest functions

·        merge()

·        tdigest\_agg()

·        value\_at\_quantile()

Teradata#

For more details, see Teradata functions

·        char2hexint()

·        index()

·        to\_char()

·        to\_timestamp()

·        to\_date()

URL#

For more details, see URL functions

·        url\_decode()

·        url\_encode()

·        url\_extract\_fragment()

·        url\_extract\_host()

·        url\_extract\_parameter()

·        url\_extract\_path()

·        url\_extract\_port()

·        url\_extract\_protocol()

·        url\_extract\_query()

UUID#

For more details, see UUID functions

·        uuid()

Window#

For more details, see Window functions

·        cume\_dist()

·        dense\_rank()

·        first\_value()

·        lag()

·        last\_value()

·        lead()

·        nth\_value()

·        ntile()

·        percent\_rank()

·        rank()

·        row\_number()

## &#x20;
