# Publishing to GitHub Pages

Once your repository is created, you can publish your comic website to GitHub Pages.

{% hint style="info" %}
Even if you've only just created your repository from the previous page, the repo comes with some default text and images as a demonstration. Feel free to publish after creating to see how it works!
{% endhint %}

1.  In your repository, go to the **Settings** tab.&#x20;

    <figure><img src="../.gitbook/assets/publish01_go_to_settings.jpg" alt="An image of the GitHub repository site, with a red box around the Settings option."><figcaption><p>The Settings option.</p></figcaption></figure>
2.  In the sidebar on the left, click **Pages**.&#x20;

    <figure><img src="../.gitbook/assets/publish02_go_to_pages.jpg" alt="An image of the GitHub repository Settings page, with a red box around the Pages option."><figcaption><p>The Pages option.</p></figcaption></figure>
3.  Under **Branch**, select `working`from the dropdown.&#x20;

    <figure><img src="../.gitbook/assets/publish03_select_branch.png" alt="An image of the GitHub Pages page, with a red box around the Branch setting."><figcaption><p>Select the working branch.</p></figcaption></figure>

{% hint style="info" %}
It takes a minute for the `working` branch to be created in a new repository, so if `working` isn't available in the dropdown, try refreshing the page.
{% endhint %}

4.  Click **Save**. Once you do, GitHub will automatically publish your repository to GitHub Pages!&#x20;

    <div align="center" data-full-width="false"><figure><img src="../.gitbook/assets/publish04_save_branch.png" alt="An image of the GitHub Pages page, with a red box around the Save button."><figcaption><p>Click Save.</p></figcaption></figure> <figure><img src="../.gitbook/assets/publish05_page_success.png" alt="An image of the GitHub Pages page, with a red box around the text &#x22;Your GitHub Pages site is currently being built from the working branch.&#x22;."><figcaption><p>Success!</p></figcaption></figure></div>
5.  Publishing isn't immediate; GitHub needs to do some work in the background to do so. For this first time, the publishing process could take a few minutes. Go get a coffee, hit the can, call your mother... and when you come back, refresh your Settings page, and you should see a new text box appear telling you that your site is now live!&#x20;

    <figure><img src="../.gitbook/assets/publish06_page_built.png" alt="An image of the GitHub Pages page, with a red box around the text &#x22;Your site is live&#x22; and the Visit Site button."><figcaption><p>It's ready!</p></figcaption></figure>

{% hint style="info" %}
You can also view the progress of your GitHub Pages deployments by looking in the Environments section of the sidebar on your main repository page.
{% endhint %}

6.  Click **Visit site** to go to your website, hosted from your very own GitHub account! WOO HOO!&#x20;

    <figure><img src="../.gitbook/assets/publish07_page_demo.jpg" alt="The live GitHub Pages site, using the default text and image."><figcaption><p>It lives!</p></figcaption></figure>

Next, we need to set up the app that will let you move your own files into the repository.

{% hint style="success" %}
## **A Tale of Two GitHub URLs**

You now have two different URLs to keep track of: Your GitHub repository URL and your GitHub Pages URL. Both of these URLs are automatically generated based on your GitHub account name and your webcomic repository name.

**Example:**

GitHub account name: **comicgitdemo** Repository name: **bestcomic**\
GitHub repository URL: https://github.com/**comicgitdemo**/**bestcomic**\
GitHub Pages URL: https://**comicgitdemo**.github.io/**bestcomic**

Your **GitHub Pages URL** is what you need to give to anyone who wants to read your comic.

If you like, you can even set a [custom domain](../additional-information/advanced-tips.md#moving-to-a-custom-domain) rather than using the default one.
{% endhint %}
