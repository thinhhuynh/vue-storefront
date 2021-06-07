# `useForgotPassword`

## Features

`useForgotPassword` composable can be used to:

* generate reset password token
* change user password using token

## API

- `reset` - function used to generate reset password token. When invoked, it requests data from the API and populates `token` property. This method accepts a single params object. The `params` has the following option:

  - `params: IResetPasswordParams`

  - `customQuery?: CustomQuery`

```typescript
interface IResetPasswordParams {
    email: string;
}

type CustomQuery = {
  customerCreatePasswordResetToken: string
}
```

- `change` - function used to set new user password after reset. When invoked, it requests data from the API and populates `result` property. This method accepts a single params object. The `params` has the following option:

  - `params: IChangePasswordParams`
  
  - `customQuery?: CustomQuery`

```typescript
interface IChangePasswordParams {
    tokenValue: string;
    newPassword: string;
}

type CustomQuery = {
  customerResetPassword: string
}
```

- `token: string` - reactive data string containing the created token.

- `loading: boolean` - reactive object containing information about loading state of `change` and `reset` methods.

- `error: UseForgotPasswordErrors` - reactive object containing the error message, if `change` or `reset` failed for any reason.

```ts
interface UseForgotPasswordErrors {
  result: Error;
}
```

## Getters

We do not provide getters for forgotPassword and its parts.

## Example

Generating reset password token and changing user password.

```vue
<template>
  <!-- Generate reset password token -->
  <form @submit.prevent="reset({ email: form.value.email })">
    <!-- form fields -->
    <button type="submit" :disabled="loading">Reset Password</button>
  </form>

  <!-- Change user password -->
  <form @submit.prevent="change({ tokenValue: <TOKEN_FROM_URL>, newPassword: form.value.password })">
    <!-- form fields -->
    <button type="submit" :disabled="loading">Save Password</button>
  </form>
  <div>{{ result }} - Boolean confirmation of successful password change</div>
</template>

<script>
import { useForgotPassword } from '@vsf-enterprise/commercetoolss';

export default {
  setup() {
    const { reset, change, result, loading } = useForgotPassword();

    return {
      reset,
      change,
      result,
      loading,
    };
  }
};

</script>
```