﻿---
uid: faking-auth
title: Faking Authentication and Authorization
---

# Faking Authentication and Authorization

bUnit comes with test specific implementation of Blazor's authentication and authorization types that makes it easy to test components that use Blazor's `<AuthorizeView>`, `<CascadingAuthenticationState>`, and `<AuthorizeRouteView>` components, and the `AuthenticationStateProvider` type.

The test implementation of Blazor's authentication and authorization can be put into the following states:

- **Authenticating**
- **Unauthenticated** and **unauthorized**
- **Authenticated** and **unauthorized**
- **Authenticated** and **authorized** 
- **Authenticated** and **authorized** with one or more **roles**, **claims**, and/or **policies**

bUnit's authentication and authorization implementation is easily available by calling [AddTestAuthorization()](xref:Bunit.FakeAuthorizationExtensions.AddTestAuthorization(Bunit.TestServiceProvider)) on a test context's `Services` collection. It returns an instance of the <xref:Bunit.TestDoubles.Authorization.TestAuthorizationContext> type that allows you to control the authentication and authorization state for a test.

The following sections will show how to set each of these states in a test.

## Setting Authenticating, Authenticated and Authorized States

The examples in the following sections will use `<UserInfo>` component listed below. It uses an injected `AuthenticationStateProvider`, the `<CascadingAuthenticationState>` and `<AuthorizeView>` components to show the user name when a user is authenticated, and it shows the authorization state, when the authenticated user is authorized.

[!code-razor[UserInfo.razor](../../../samples/components/UserInfo.razor)]

The following subsections will demonstrate how to set the `<UserInfo>` into all three authentication and authorization states.

### Unauthenticated and Unauthorized State

To set the state to unauthenticated and unauthorized, do the following:

[!code-csharp[UserInfoTest.cs](../../../samples/tests/xunit/UserInfoTest.cs?start=11&end=20&highlight=3)]

The highlighted line shows how `AddTestAuthorization()` is used to add the test specific implementation of Blazor's authentication and authorization types to the `Services` collection, which makes authentication state available to other services and components used throughout the test that requires it.

After calling `AddTestAuthorization()`, the default authentication state is unauthenticated and unauthorized.

### Authenticating and Authorizing State

To set the state to authenticating and authorizing, do the following:

[!code-csharp[UserInfoTest.cs](../../../samples/tests/xunit/UserInfoTest.cs?start=26&end=36&highlight=4)]

After calling `AddTestAuthorization()`, the returned <xref:Bunit.TestDoubles.Authorization.TestAuthorizationContext> is used to set the authenticating and authorizing state through the <xref:Bunit.TestDoubles.Authorization.TestAuthorizationContext.SetAuthorizing> method.

### Authenticated and Unauthorized State

To set the state to authenticated and unauthorized, do the following:

[!code-csharp[UserInfoTest.cs](../../../samples/tests/xunit/UserInfoTest.cs?start=42&end=52&highlight=4)]

After calling `AddTestAuthorization()`, the returned <xref:Bunit.TestDoubles.Authorization.TestAuthorizationContext> is used to set the authenticated and unauthorized state through the <xref:Bunit.TestDoubles.Authorization.TestAuthorizationContext.SetAuthorized(System.String,Bunit.TestDoubles.Authorization.AuthorizationState)> method.

### Authenticated and Authorized state

To set the state to authenticated and authorized, do the following:

[!code-csharp[UserInfoTest.cs](../../../samples/tests/xunit/UserInfoTest.cs?start=58&end=68&highlight=4)]

After calling `AddTestAuthorization()`, the returned <xref:Bunit.TestDoubles.Authorization.TestAuthorizationContext> is used to set the authenticated and authorized state through the <xref:Bunit.TestDoubles.Authorization.TestAuthorizationContext.SetAuthorized(System.String,Bunit.TestDoubles.Authorization.AuthorizationState)> method. 

Note, the second parameter, `AuthorizationState`, is optional, and defaults to `AuthorizationState.Authorized`, if not specified.

## Setting Authorization Details

The following section will show how to specify **roles** and/or **policies** in a test.

The examples will use the `<UserRights>` component listed below. It the `<AuthorizeView>` components to include different content based on the **roles**, **claims**, or **policies** specified in each test.

[!code-razor[UserRights.razor](../../../samples/components/UserRights.razor)]

### Roles

To specify one or more roles for the authenticated and authorized user, do the following:

[!code-csharp[UserRightsTest.cs](../../../samples/tests/xunit/UserRightsTest.cs?start=29&end=42&highlight=5)]

The highlighted line shows how the <xref:Bunit.TestDoubles.Authorization.TestAuthorizationContext.SetRoles(System.String[])> method is used to specify one role. To specify multiple roles, do the following:

[!code-csharp[UserRightsTest.cs](../../../samples/tests/xunit/UserRightsTest.cs?start=48&end=62&highlight=5)]

### Policies

To specify one or more policies for the authenticated and authorized user, do the following:

[!code-csharp[UserRightsTest.cs](../../../samples/tests/xunit/UserRightsTest.cs?start=68&end=81&highlight=5)]

The highlighted line shows how the <xref:Bunit.TestDoubles.Authorization.TestAuthorizationContext.SetPolicies(System.String[])> method is used to specify one policy. To specify multiple policies, do the following:

[!code-csharp[](../../../samples/tests/xunit/UserRightsTest.cs?start=91&end=91)]

### Claims

To specify one or more claims for the authenticated and authorized user, do the following:

[!code-csharp[UserRightsTest.cs](../../../samples/tests/xunit/UserRightsTest.cs?start=106&end=123&highlight=5-8)]

The highlighted line shows how the <xref:Bunit.TestDoubles.Authorization.TestAuthorizationContext.SetClaims(System.Security.Claims.Claim[])> method is used to pass two instances of the `Claim` types.

### Example of passing both roles, claims, and policies

The following example specifies two roles, one claim, and one policy for the authenticated and authorized user:

[!code-csharp[UserRightsTest.cs](../../../samples/tests/xunit/UserRightsTest.cs?start=129&end=147&highlight=5-8)]