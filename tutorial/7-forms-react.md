---
pos: 7
title: 7. Forms Handling in React.js
description: >
  Using React.js to handle form validations & submissions
---

# Forms with React.js

We can improve the input field by transforming it into a real, fully functional, non-static form. We will be using `react-hook-form` to control the form behavior. `react-hook-form` is included in the Kretes React.js template. Thus, there is no need to add it as a dependency to your project.

Let's start by *registering* our single input field as being managed via `react-hook-form` by using `register` - this function comes from the `useForm` hook. Additionally, you must give the input field a name. In the example, we name it `name`.

The logic that will be executed once someone clicks the add button is defined in the `onSubmit` function. Here, we wrap our custom logic as an anonymous function passed to `handleSubmit` that comes from the `useForm` hook.

```tsx{2,5-6,9-10}
import React from 'react';
import { useForm } from "react-hook-form";

export const TaskInput: React.FC<{}> = () => {
  const { register, handleSubmit } = useForm();
  const onSubmit = handleSubmit(data => console.log(data))

  return (
    <form className="flex items-center justify-between relative mb-8" onSubmit={onSubmit}>
      <input
        name="name"
        ref={register}
        placeholder="Add new item..."
        type="text"
        className="p-4 pr-20 border-l-4 border-gray-500 bg-gray-200 w-full shadow-inner outline-none"
      />
      <button type="submit" className="shadow text-blue-100 border-blue-100 bg-gray-500 font-semibold py-2 px-4 absolute right-0 mr-2">Add</button>
    </form>
  );
}
```

Optionally, we can mark the input field as required. This way, it won't be possible to submit empty tasks. Use the `required` attribute as the `register` parameter.

```tsx{10}
import React from 'react';
import { useForm } from "react-hook-form";

export const TaskInput: React.FC<{}> = () => {
  const { register, handleSubmit } = useForm();
  const onSubmit = handleSubmit(data => console.log(data))

  return (
    <form className="flex items-center justify-between relative mb-8" onSubmit={onSubmit}>
      <input
        name="name"
        ref={register({ required: true })}
        placeholder="Add new item..."
        type="text"
        className="p-4 pr-20 border-l-4 border-gray-500 bg-gray-200 w-full shadow-inner outline-none"
      />
      <button type="submit" className="shadow text-blue-100 border-blue-100 bg-gray-500 font-semibold py-2 px-4 absolute right-0 mr-2">Add</button>
    </form>
  );
}
```

To display possible errors, we need to use the `errors` object that comes from the `useForm` hook. It contains not `null` values for any fields that are registered in our form.


```tsx{5,11}
import React from 'react';
import { useForm } from "react-hook-form";

export const TaskInput: React.FC<{}> = () => {
  const { register, handleSubmit, errors } = useForm();
  const onSubmit = handleSubmit(data => console.log(data))

  return (
    <form className="flex items-center justify-between relative mb-8" onSubmit={onSubmit}>
      <input
        name="name"
        ref={register({ required: true })}
        placeholder="Add new item..."
        type="text"
        className="p-4 pr-20 border-l-4 border-gray-500 bg-gray-200 w-full shadow-inner outline-none"
      />
      {errors.name && <span>This field is required</span>}
      <button type="submit" className="shadow text-blue-100 border-blue-100 bg-gray-500 font-semibold py-2 px-4 absolute right-0 mr-2">Add</button>
    </form>
  );
}
```
