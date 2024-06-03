# Unlike All Facebook Pages from Activity

## Instructions

1. Go to [Facebook Activity Log](https://www.facebook.com/usrID/allactivity?activity_history=false&category_key=LIKEDINTERESTS&manage_mode=false&should_load_landing_page=false).

2. Open the console in your browser (F12 -> Console).

3. Paste the following script into the console and press Enter:

    ```javascript
    function unlikeAllPages() {
        const actionButtons = document.querySelectorAll('[aria-label="Action options"]');
        let index = 0;
        let retryCount = 0;
        const maxRetries = 3;

        function clickActionButtons() {
            if (index < actionButtons.length) {
                try {
                    actionButtons[index].click();
                    setTimeout(() => {
                        try {
                            const menuItems = document.querySelectorAll('[role="menuitem"]');
                            let unliked = false;
                            menuItems.forEach((item) => {
                                if (item.innerText.indexOf('Unlike') !== -1) {
                                    item.click();
                                    unliked = true;
                                }
                            });

                            if (unliked) {
                                retryCount = 0;
                                index++;
                                setTimeout(clickActionButtons, 300);
                            } else {
                                handleRetry();
                            }
                        } catch (error) {
                            handleRetry();
                        }
                    }, 300);
                } catch (error) {
                    handleRetry();
                }
            } else {
                window.scrollTo(0, document.body.scrollHeight);
                setTimeout(() => {
                    unlikeAllPages();
                }, 1000);
            }
        }

        function handleRetry() {
            if (retryCount < maxRetries) {
                retryCount++;
                setTimeout(clickActionButtons, 300);
            } else {
                retryCount = 0;
                index++;
                setTimeout(clickActionButtons, 300);
            }
        }

        clickActionButtons();
    }

    unlikeAllPages();
    ```
