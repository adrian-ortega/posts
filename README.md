## Entity types



### Attributes
All entity attributes are held at the head of the `.md` corresponding document and are organized using the [YAML format](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html).


### Tags

File name format: `tag.<slug>.md`

#### Attributes
| Name | Required | Type | Description |
| ---- | -------- | ---- | ----------- |
| `name` | Yes | *String* | The full name of the Tag |
| `description` | No | *String* | The tag description, this can hold HTML |

#### Example
File name: `tag.code-examples.md`
```md
---
name: Code Examples
description: Quick code tutorials you can do yourself
---
```

### Posts

File name format: `post.<slug>.md`

#### Attributes
| Name | Required | Type | Description |
| ---- | -------- | ---- | ----------- |
| `title` | **Yes** | *String* | The main title of the post. This is also the fallback  when `page_title` is not passed. |
| `created_at` | **Yes** | *String* | The date this post was created, will be used as the `published_at` Date if it's missing |
| `page_title` | No | *String* | Optional, this will override the post title in the `<head/>` of the resulting document/post |
| `description` | No | *String* | The short description for the post |
| `tags` | No | *Array* | A list of slugs to tag the post with |
| `thumbnail` | No | *String* | A small image used as the thumbnail |
| `cover_image` | No | *String* | The main image associated with the post |
| `published_at` | No | *String* | This will override the created_at timestamp |
| `updated_at` | No | *String* | This will allow the front end to display an "updated at" message, in case the post has new content |
| `reading_time` | No | *String*  | Description |
| `draft` | No | *Boolean* | Is this in Draft mode? all posts in draft will not be displayed |
| `featured` | No | *Boolean* | Show this as a the featured post in the homepage? |
| `author` | No | *String* | The name of the author |

#### Example
File name: `post.example-post.md`
```md
---
title: Example Code
description: A quick example of what to do when you want to display a post on the frontend
created_at: "2025-01-31T12:00:00Z"
tags: ["code-examples", "javascript"]
featured: false
---

# Example Code

A quick example of what to do, this is a sentence merely used as an example of text.

## Heading 2

Another sentence
```