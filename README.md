更新了Esc停止的功能
 const minInterval:更改點擊速率
 const maxDuration:更改點擊時長
 
    
    (function() {
    // 使用客戶端可見區域尺寸，而不是內部尺寸，避免工具列影響
    const width = document.documentElement.clientWidth;
    const height = document.documentElement.clientHeight;
    const centerX = width / 2;
    const centerY = height / 2;

    const minInterval = 200;
    const maxInterval = 500;
    let clicks = 0;
    const maxDuration = 500 * 1000; // 60秒
    let isRunning = true;

    console.log("自動點擊開始，將在可見視窗中心執行...");
    console.log("按下 'Esc' 鍵可隨時停止");

    // 監聽 Esc 鍵
    document.addEventListener('keydown', (event) => {
        if (event.key === 'Escape') {
            isRunning = false;
            console.log("收到 Esc 鍵，已停止自動點擊");
        }
    });

    // 顯示紅點的函數
    function showRedDot(x, y) {
        const dot = document.createElement('div');
        dot.style.position = 'absolute';
        dot.style.left = `${x - 5}px`;
        dot.style.top = `${y - 5}px`;
        dot.style.width = '10px';
        dot.style.height = '10px';
        dot.style.backgroundColor = 'red';
        dot.style.borderRadius = '50%';
        dot.style.zIndex = '9999';
        document.body.appendChild(dot);

        setTimeout(() => {
            document.body.removeChild(dot);
        }, 500);
    }

    function clickAtCenter() {
        if (!isRunning) return;

        // 考慮滾動偏移，計算絕對坐標
        const scrollX = window.scrollX || window.pageXOffset;
        const scrollY = window.scrollY || window.pageYOffset;
        const xOffset = Math.floor(Math.random() * 21) - 10;
        const yOffset = Math.floor(Math.random() * 21) - 10;
        const clickX = centerX + xOffset + scrollX;
        const clickY = centerY + yOffset + scrollY;

        // 模擬點擊
        const clickEvent = new MouseEvent('click', {
            bubbles: true,
            cancelable: true,
            view: window,
            clientX: clickX,
            clientY: clickY
        });
        const element = document.elementFromPoint(clickX - scrollX, clickY - scrollY);
        if (element) {
            element.dispatchEvent(clickEvent);
        }

        // 顯示紅點（使用客戶端坐標）
        showRedDot(clickX - scrollX, clickY - scrollY);

        clicks++;
        console.log(`已點擊 ${clicks} 次`);

        const interval = Math.random() * (maxInterval - minInterval) + minInterval;
        if (Date.now() - startTime < maxDuration && isRunning) {
            setTimeout(clickAtCenter, interval);
        } else {
            console.log("自動點擊結束");
        }
    }

    const startTime = Date.now();
    clickAtCenter();
    })();
