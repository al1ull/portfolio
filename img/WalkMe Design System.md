# WalkMe Design System

This page lists our library of solutions using WalkMe, when to use them, and how to implement them using templates.

Style are documented here: <https://www.figma.com/design/TbKCzB8bg8L14JHBE9yj5y/WalkMe?node-id=552-30386&p=f&m=dev> 

# When to use what

| Type | User need | Preferred guidance | Typical trigger |
| --- | --- | --- | --- |
| Onboarding | Accomplish a meaningful task for the first time | [Tutorial](https://streetlightdata.atlassian.net/wiki/spaces/CS/pages/edit-v2/4459429889#Tutorials)s, [Tours](https://streetlightdata.atlassian.net/wiki/spaces/CS/pages/edit-v2/4459429889#Tour), [Help Me Choose](https://streetlightdata.atlassian.net/wiki/spaces/CS/pages/edit-v2/4459429889#Help-Me-Choose-Modal) | First encounter with a major workflow or user request |
| Ongoing | Complete new or infrequent tasks | [Tutorials](https://streetlightdata.atlassian.net/wiki/spaces/CS/pages/edit-v2/4459429889#Tutorials), [Learn More](https://streetlightdata.atlassian.net/wiki/spaces/CS/pages/edit-v2/4459429889#Learn-More-Modal) | User action or request |
| Reference | Understand a concept, metric, or methodology | [Learn more](https://streetlightdata.atlassian.net/wiki/spaces/CS/pages/edit-v2/4459429889#Learn-More-Modal), [tooltips](https://streetlightdata.atlassian.net/wiki/spaces/CS/pages/edit-v2/4459429889#Tooltips), Help Center, training | User requests more information |
| New features | Discover new capabilities or understand changed workflows | [What's New](https://streetlightdata.atlassian.net/wiki/spaces/CS/pages/edit-v2/4459429889#What%E2%80%99s-New-Module), [New Feature popups](https://streetlightdata.atlassian.net/wiki/spaces/CS/pages/edit-v2/4459429889#New-Feature-Popups) | Relevant release or UI change |
| Operational | Understand a problem that affects their work | [known issue popup](https://streetlightdata.atlassian.net/wiki/spaces/CS/pages/edit-v2/4459429889#Known-Issue-Popup), [maintenance banner](https://streetlightdata.atlassian.net/wiki/spaces/CS/pages/edit-v2/4459429889#Maintenance-banner) | Relevant condition or service event |

Guidance should be selected based on the user's current need, not simply whether they are new to the product. Experienced users may need onboarding when encountering unfamiliar workflows, and new users may not need guidance for familiar tasks.

# Tutorials

|  |  |
| --- | --- |
| **When to use** | Guide users through a real workflow that helps them accomplish a meaningful task, such as creating an analysis, comparing travel times, or exploring visualization results. |
| **Usage guidelines** | Aim for 3–6 meaningful steps; use more only when necessary.Organize steps around the user's task, not every control in the interface.Use clear, action-oriented titles.Explain why important choices matter, not just where to click.Advance when users complete actions, where reliable.Skip completed or irrelevant steps when possible.Keep detailed definitions and advanced instructions in contextual help.Don't automatically launch another tutorial when one finishes.End with a meaningful outcome and an optional next step. |
| **How to implement** | Default Smart Walk-Thru style |
| **Location in system** | Relevant folder for UI location |
| **WalkMe Element Type** | Smart Walk-Thrus |

# Tour

|  |  |
| --- | --- |
| **When to use** | Use an orientation tour only when users need to understand the layout or organization of a complex interface before they can work effectively. Prefer task-based tutorials when users have a specific goal. |
| **Usage guidelines** | Keep tours brief, generally 2–4 meaningful steps.Limit tours to one page or interface.Make tours available on demand; do not automatically launch them.Introduce only controls essential to getting started.Avoid explaining every feature or navigation element. |
| **How to implement** | Default Smart Walk-Thru style |
| **Location in system** | Relevant folder for UI location |
| **WalkMe Element Type** | Smart Walk-Thru |

# Welcome Modal


|  |  |
| --- | --- |
| **When to use** | Use a full Welcome Modal only when users need a substantial introduction or must acknowledge an important product change before continuing. For ordinary first-use onboarding and new feature announcements, prefer a compact, nonblocking invitation. |
| **Usage guidelines** | Do not use a full Welcome Modal as the default introduction to a new module or page.Do not require users to watch a video before working.Use the First-Use Invitation pattern for optional walkthroughs.Reserve automatic full-screen or blocking experiences for justified exceptions.Provide a clear dismissal option when the content is optional. |
| **How to implement** | 1. Create a new ShoutOut 2. Select *Welcome Modal Template*  3. Configure Autoplay to appear Once at the url |
| **Location in system** | Relevant folder for UI location |
| **WalkMe Element Type** | ShoutOut |

# New Feature Popups


|  |  |
| --- | --- |
| **When to use** | To call attention to a new, updated, or changed feature in the UI |
| **Usage guidelines** | Use for significant UI changes or capabilities users might otherwise overlook.Anchor the popup to the relevant control when possible.Keep to one short title, one or two sentences, and a clear action.Avoid full-screen overlays.Show only to users affected by the change.Suppress after dismissal or completion.Avoid duplicating information already promoted through What's New.Never display alongside another automatic onboarding invitation.Make detailed information available on demand. |
| **How to implement** | 1. Create a new Smart Walk-Thru 2. Create a step 3. Edit the step 4. Go to the *Design* tab 5. From the Design panel on the right, scroll to the bottom and select the circular icon by *CSS & Layout* 6. Select *New Feature Popup* 7. Fill out the text for the step (the template will overwrite anything you’ve already entered). 8. Configure Autoplay to appear Once. |
| **Location in system** | Pub - New feature popups |
| **WalkMe Element Type** | Smart Walk-Thru |

# What’s New Module


|  |  |
| --- | --- |
| **When to use** | Available from the What’s New button, After a feature release, this slideshow shows information about new features with relevant links to more information. Developed with content from Product Marketing’s Product Update email |
| **Usage guidelines** | Follow guidelines in the templatesLinks to the *See What’s New* launcher in the UIThe What's New carousel is always available from the What's New launcher but should not automatically open. Instead, a compact, nonblocking release invitation may appear once per release to direct users to the launcher. |
| **How to implement** | Edit the existing slides in the *What’s New Module* folder. To create a new slide, copy the templates in the folder.Configure Autoplay on the Welcome slide to appear Once. |
| **Location in system** | **Planning**: Pub - What’s New Module**Operations**: Pub - Operations Solutions \> OS What's New |
| **WalkMe Element Type** | ShoutOuts |

# Learn More Modal

|  |  |
| --- | --- |
| **When to use** | Use a Learn More Modal when a brief explanation helps users understand a concept without leaving their workflow. Link directly to the Help Center when users need detailed procedures, reference material, or methodology information. |
| **Usage guidelines** | Adjust the location on the screen if the user needs to see the UI |
| **How to implement** | 1. Have development add a unique html id to *Learn more* text in the UI 2. Create a Launcher over that link, using the \[Launcher template\]. 3. Create a new ShoutOut with the Inform Modal template. 4. Link the Launcher to the ShoutOut |
| **Location in system** | Relevant folder for UI location |
| **WalkMe Element Type** | ShoutOut/Launcher |

# Help Me Choose Modal

|  |  |
| --- | --- |
| **When to use** | Help users choose between options when the appropriate selection depends on their transportation question, intended outcome, or study requirements. |
| **Usage guidelines** | Start with the user's question or goal.Present a small number of understandable choices.Recommend the most appropriate option and briefly explain why.Provide a direct action to continue with that option.Ask additional questions only when necessary.Consider data availability, subscriptions, and methodological restrictions.Link to detailed comparisons in the Help Center.Avoid presenting long descriptions of every available option. |
| **How to implement** | 1. Have development add a unique html id to *Help me choose* text in the UI 2. Create a Launcher over that link, using the \[Launcher template\]. 3. Create a new ShoutOut with the Inform Modal template. 4. Link the Launcher to the ShoutOut |
| **Location in system** | Relevant folder for UI location |
| **WalkMe Element Type** | ShoutOut/Launcher |


# Tooltips

|  |  |
| --- | --- |
| **When to use** | To provide a definition, caveat, or detail for an element or section in the UI. |
| **Usage guidelines** | If a user needs to know something to use the app, it should be in the UI, not a tooltip.Tooltips can have *Learn more* links that open a Learn More ModalIn WalkMe, SmartTips are organized into SmartTip sets. Check if the page where you want to add a tooltip already has an associated SmartTip set. |
| **How to implement** | 1. Have development add a unique html id to an ( i ) icon in the UI 2. Create a Launcher over that link, using the ( i ) (invisible) launcher template. 3. Create a new SmartTip, with the jquery html id as the selected element. |
| **Location in system** | Relevant folder for UI location |
| **WalkMe Element Type** | SmartTip |

# Known Issue Popup

|  |  |
| --- | --- |
| **When to use** | To notify a user that they may encounter an error as a result of a known issue, to provide a workaround, and to let them know we are aware. |
| **Usage guidelines** | Work closely with Product Management to design behavior and content. |
| **How to implement** | 1. Create a new Smart Walk-Thru 2. Create a step 3. Edit the step 4. Go to the *Design* tab 5. From the Design panel on the right, scroll to the bottom and select the circular icon by *CSS & Layout* 6. Select *Known Issue* 7. Fill out the text for the step (the template will overwrite anything you’ve already entered). 8. Use autoplay and wait for rules to automate appearance |
| **Location in system** | Bug Fixes/Known Issues |
| **WalkMe Element Type** | Smart Walk-Thru |

# Maintenance banner

\[Image to be updated with new design\]

|  |  |
| --- | --- |
| **When to use** | To notify a users that the platform or product will be down for maintenance. Done before each quarterly release deployment. Occasionally used for other types of maintenance or disruptions. |
| **Usage guidelines** | Coordinate with the ops team to ensure time and language is right |
| **How to implement** | 1. Copy the *Maintenance Banner (OPS) *ShoutOut 2. Adjust the autoplay to be Once per day, starting when QA signs off on DR testing  3. Remove the banner when the deployment itself starts |
| **Location in system** | Promos & Notifications \> Alerts & Notifications |
| **WalkMe Element Type** | ShoutOut |

# Special Cases/One-offs

## Events and announcements popup

|  |  |
| --- | --- |
| **When to use** | Update after each training or webinar with the next 3 events in the calendar |
| **Usage guidelines** | Use for: - Public Trainings - Quarterly Release Webinars - StreetLight Labs/Surveys? |
| **How to implement** | 1. Update the existing “Events & Announcements” shoutout, with the soonest event at the top. 2. Update the “Register Now” buttons with the appropriate links |
| **Location in system** | Promos & Notifications \> Promos |
| **WalkMe Element Type** | ShoutOut |

## Surveys


|  |  |
| --- | --- |
| **When to use** | Deploy to link to the yearly NPS Survey organized by customer careWork with product for any other surveys on betas and vew product launches |
| **Usage guidelines** | To deploy a survey, users should launch it from a banner, rather than autoplaying the survey itself |
| **How to implement** | 1. Create a new shoutout and use the “Survey banner” template 2. Update the text with the subject of the survey 3. Create surveys at <https://console.walkme.com/surveys/list>  *NOTE: work with the WalkMe lead to update the Global CSS so the colors of the survey work. This is required until WalkMe updates dark mode/light mode functionality * |
| **Location in system** | Promos & Notifications \> InSight Forms and Surveys |
| **WalkMe Element Type** | ShoutOut + Survey |

## Onboarding Materials

|  |  |
| --- | --- |
| **When to use** | Appears the first time a user enters a new product |
| **Usage guidelines** | Ensure autoplay is once for user sessions where last seen does not existConnect to a relevant onboarding quick tour or walkthroughGreat guidelines here: <https://spectrum.adobe.com/page/writing-for-onboarding/> |
| **How to implement** |  |
| **Location in system** |  |
| **WalkMe Element Type** | ShoutOut \> Smart Walkthru |

## Table of Contents

## UI Term Glossary

These terms will be used below to describe components 

| **Term** | **Definition** |
| --- | --- |
| Modal | A window that appears over the app in response to a user action |
| Popup | A box that automatically appears over the app |
| Tooltip | A small box with text that appears when the user hovers over a UI element like an ( i ) icon. |
| Balloon | A box that appears next to an element in the UI, with an arrow pointing at the element. |

## Related Links

[What is WalkMe? (for PM/Dev/Design)](https://streetlightdata.atlassian.net/wiki/spaces/CS/pages/4394156092/What+is+WalkMe+for+PM+Dev+Design)

<https://www.figma.com/design/TbKCzB8bg8L14JHBE9yj5y/WalkMe?node-id=283-3036&p=f&m=dev> 

## Artifacts

Old WalkMe CSS:
