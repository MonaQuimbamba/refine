---
id: add-edit-page
title: 2. Adding Edit Page
tutorial:
    order: 0
    prev: tutorial/adding-crud-pages/{preferredUI}/index
    next: tutorial/adding-crud-pages/{preferredUI}/add-show-page
---

Edit page is the page where you can edit the record. In this tutorial, we will create the edit page for the `products` resource.

## Creating Edit Page

First, let's create our file under the `src/pages/products` folder. We will name it `edit.tsx`. Then, we will copy the edit page code generated by Inferencer and paste it into the file.

To copy the code and paste it into the file, follow the steps below:

1. Navigate to the <a href="http://localhost:3000/products" rel="noopener noreferrer nofollow">localhost:3000/products</a> in your browser.

2. To open the edit page, click any "Edit" button in the "Actions" column of the table.

3. On the edit page, click on the "Show Code" button in the bottom right corner of the page.

4. You can see the edit page code generated by Inferencer. Click on the "Copy" button to copy the code.

5. Paste the code into the you created, `edit.tsx` file.

You can see the edit page code generated by Inferencer below:

```tsx live previewOnly previewHeight=600px url=http://localhost:3000/products/edit/123
setInitialRoutes(["/products/edit/123"]);

import { Refine } from "@pankod/refine-core";
import routerProvider from "@pankod/refine-react-router-v6";
import dataProvider from "@pankod/refine-simple-rest";
import { HeadlessInferencer } from "@pankod/refine-inferencer/headless";

const App = () => {
    return (
        <Refine
            routerProvider={routerProvider}
            dataProvider={dataProvider("https://api.fake-rest.refine.dev")}
            resources={[
                {
                    name: "products",
                    list: HeadlessInferencer,
                    show: HeadlessInferencer,
                    create: HeadlessInferencer,
                    edit: HeadlessInferencer,
                },
            ]}
        />
    );
};
render(<App />);
```

Instead of coding the edit page component from scratch, Inferencer created the required code base on API response, so that we can customize.

## Understanding the Edit Component

We will go through the list page hooks one by one.

-   `useForm` hook, imported from `@pankod/refine-react-hook-form` package, has been developed by using the **React Hook Form** and `useForm` hook imported from `@pankod/refine-core` package.

    It provides all the features of the `useForm` hook from `@pankod/refine-core` package as well as the `useForm` hook from **React Hook Form**.

    It also provides the `saveButtonProps` prop that we can pass to the submit button of the form.

    When you use `useForm` in the edit page, it automatically fetches the data of the record by using the `id` in the URL, then fills the form with the data. It sends the form data to `dataProvider`'s `update` method when the form is submitted.

    [Refer to the **@pankod/refine-react-hook-form** `useForm` documentation for more information &#8594](/docs/packages/documentation/react-hook-form/useForm/)

    [Refer to the **React Hook Form** documentation for more information &#8594](https://react-hook-form.com/)

-   `useNavigation` is a **refine** hook that is used to navigate between pages. In this case, we are using it to navigate to the `list` page when user clicks the "Products List" button.

    [Refer to the `useNavigation` documentation for more information &#8594](/docs/api-reference/core/hooks/navigation/useNavigation/)

### Handling Relationships

In the edit page, we may need to select a record from another resource. For example, we may need to select a category from the `categories` resource to assign the product to the category. In this case, we can use the `useSelect` hook provided by **refine**. This hook fetches the data by passing the params to the `dataProvider`'s `getList` method. Then, it returns the `options` to be used in the `<select/>` component.

[Refer to the `useSelect` documentation for more information &#8594](/docs/api-reference/core/hooks/useSelect/)

In the auto-generated edit page code, Inferencer used the `useSelect` hook to select a category from the `categories` resource like below:

```tsx
const { options: categoryOptions } = useSelect({
    resource: "categories",
});
```

`useSelect` returns 10 record by default, but the category of the product may not be in the first 10 records. To solve this problem, we can use the `defaultValue` prop to set the default value of the `useSelect` hook like below:

```tsx
const { options: categoryOptions } = useSelect({
    resource: "categories",
    defaultValue: productsData?.category?.id,
});
```

## Adding the Edit Page to the App

Now that we have created the edit page, we need to add it to the `App.tsx` file.

1. Open `src/App.tsx` file on your editor.

2. Import the created `ProductEdit` component.

3. Replace the `HeadlessInferencer` component with the `ProductEdit` component.

```tsx title="src/App.tsx"
import { Refine } from "@pankod/refine-core";
import routerProvider from "@pankod/refine-react-router-v6";
import dataProvider from "@pankod/refine-simple-rest";
import { HeadlessInferencer } from "@pankod/refine-inferencer/headless";

import { ProductList } from "pages/products/list";
//highlight-next-line
import { ProductEdit } from "pages/products/edit";

const App = () => {
    return (
        <Refine
            routerProvider={routerProvider}
            dataProvider={dataProvider("https://api.fake-rest.refine.dev")}
            resources={[
                {
                    name: "products",
                    list: ProductList,
                    //highlight-next-line
                    edit: ProductEdit,
                    show: HeadlessInferencer,
                    create: HeadlessInferencer,
                },
            ]}
        />
    );
};
export default App;
```

Now, we can see the edit page in the browser at <a href="http://localhost:3000/products/edit/123" rel="noopener noreferrer nofollow">localhost:3000/products/edit/123</a>

<br/>
<br/>

<Checklist>

<ChecklistItem id="add-edit-page-headless">
I added the edit page to the app.
</ChecklistItem>
<ChecklistItem id="add-edit-page-headless-2">
I understood the edit page components and hooks.
</ChecklistItem>
<ChecklistItem id="add-edit-page-headless-3">
I understood the relationship handling.
</ChecklistItem>

</Checklist>