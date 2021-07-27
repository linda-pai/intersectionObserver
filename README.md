# intersectionObserver

** 用途1:** 

由於該頁面圖片多，為提升loading速度以及使用者體驗，以Intersection Observer偵測該目標是否在可見位置，

一開始圖片為一預設圖片，

當目標和intersection observer相交時，再載入真正的圖片。

```
//延遲載入圖片Lazy-loading
let options = {
        root: null,
        rootMargin: '300px',
        threshold: 0.1
    }
    
const lazyLoadImages = (entries, observer) => {

        entries.map(entry => {
            if (entry.isIntersecting) {
                entry.target.src = entry.target.getAttribute('data-src');
            }
            return null
        })
    }
    
    let observer = new IntersectionObserver(lazyLoadImages, options);
    let observTargets = document.querySelectorAll('.lazy-load');

    observTargets.forEach(target => observer.observe(target))
    
```

** 用途2:** 

（類似facebook作法）

頁面一開始只有一頁(8筆的資料)

當滑鼠畫到底部時，再抓取下一頁資料

```

//觀察到底部loading字-觸發fetch頁數+1
    const lazyLoadImages = (entries) => {
        const y = entries[0].boundingClientRect.y;
        if (prevY > y) {
            setLoading(true)
            if (page + 1 <= maxPage) {
                setPage(page + 1);
            } else {
                setPage(maxPage)
            }
        } else {
            setLoading(false)
        }
        setPrevY(y);
    };
    
 //滑至底部顯示更多-設定觀察範圍(底部loading字)
    useEffect(() => {
        const options = {
            root: null,
            rootMargin: '0px',
            threshold: 0.25
        }
        const observer = new IntersectionObserver(lazyLoadImages, options);

        if (loadingRef && loadingRef.current) {
            observer.observe(loadingRef.current)
        }
        return () => observer.unobserve(loadingRef.current);

    }, [loadingRef, lazyLoadImages])
    
```
