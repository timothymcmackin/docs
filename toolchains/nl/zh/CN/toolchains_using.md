---

copyright:
  years: 2016

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# 使用工具链
{: #toolchains-using}

*上次更新时间：2016 年 5 月 4 日*
{: .last-updated}

您可以使用工具链，以使您的每日开发、部署和操作工作更富成效。设置工具链之后，您可以添加、删除或配置工具集成，并管理工具链的访问。
{: shortdesc}

**重要信息**：此功能是试验性的。工具链可能不稳定，并且可能会发生变化而与较低版本不兼容。建议不要在生产环境中使用工具链。要使用工具链，您必须进行一次性的[访问请求](https://new-console.ng.bluemix.net/devops?cm_mmc=IBMBluemixGarageMethod-_-MethodSite-_-10-19-15::12-31-18-_-toolchains-welcome-page){: new_window}。工具链仅在美国南部区域可用。

## 配置工具集成
{: #configuring_a_tool_integration}

您可以全新配置工具集成，或更新已经是工具链一部分的工具集成的配置设置。

1. 在 DevOps 仪表板的**工具链**选项卡上，单击工具链，以打开其“工具集成”页面。或者，在应用程序“概述”页面的“持续交付”磁贴上，单击**查看工具链**，然后单击**工具集成**。
1. 如果您需要全新配置工具集成，请在其磁贴上，单击**配置**。

  ![“配置”按钮](images/toolchain_tile_configure.png)

 完成配置工具集成时，单击**保存集成**。
 
1. 如果您需要更新工具集成的配置，请在其磁贴上，单击菜单以访问配置选项。

  ![配置菜单](images/toolchain_tile_menu.png)
 
 完成更新设置时，单击**保存集成**。

 **注**：为 GitHub 工具集成配置存储库之后，可以更新该存储库的 URL，但是无法更改存储库本身。要使用不同的存储库，请从工具链删除当前 GitHub 工具集成，向工具链添加 GitHub 工具集成，然后配置该工具集成以使用新的存储库。

## 添加工具集成
{: #adding_a_tool_integration}

您可以为工具链添加和配置工具集成。

1. 在 DevOps 仪表板的**工具链**选项卡上，单击工具链，以打开其“工具集成”页面。或者，在应用程序“概述”页面的“持续交付”磁贴上，单击**查看工具链**，然后单击**工具集成**。
1. 要查看要添加的工具集成列表，请单击添加按钮 (+)。
1. 单击要添加的工具集成。
1. 输入配置工具集成所需的任何信息。 
1. 单击**创建集成**，以向工具链添加工具集成。

## 删除工具集成
{: #deleting_a_tool_integration}

您可以从工具链删除工具集成。 

1. 在 DevOps 仪表板的**工具链**选项卡上，单击工具链，以打开其“工具集成”页面。或者，在应用程序“概述”页面的“持续交付”磁贴上，单击**查看工具链**，然后单击**工具集成**。
1. 在要删除的工具集成的磁贴上，单击菜单以访问配置选项。
1. 要从工具链删除工具集成，请单击**删除**。
1. 单击**删除**以确认。

## 管理访问权
{: #managing_access}

您可以通过将用户添加到与工具链相关联的组织，对用户授予工具链的访问权。每一个工具链都与特定组织相关联，且属于该组织成员的任何用户都可以访问相关联的工具链。如果您切换到不同的组织，那么您可以访问不同的工具链集。

<!--CA: Commenting out the content on authentication for Interconnect since it applies to GitHub Enterprise. This content can be exposed again when GHE is supported for the Dedicated Beta 2.-->

<!--You have three authentication options for your Bluemix dedicated environment: LDAP, SAML, or Web ID. 

**Important:** For this beta, Web ID authentication requires additional user management on GitHub Enterprise.

If you use LDAP or SAML authentication in your Bluemix dedicated environment, when you add users to your Bluemix org and spaces, the users can log in to GitHub Enterprise by using their Bluemix ID and password, and accounts are created for them. When you add users to your Bluemix org and spaces, they are not automatically added to the GitHub Enterprise repo. Someone who has admin privileges for the repo must add them.  

If you use Web ID authentication, when you add users to your Bluemix org and spaces, a GitHub Enterprise site administrator must set up a GitHub Enterprise account for those users. Alternatively, new users can create a toolchain, in which case a GitHub Enterprise account is created for them. However, if those users want to access repos that are associated with toolchains besides their own, they must be granted access to those repos.

To add a user: -->

1. 在 DevOps 仪表板的**工具链**选项卡上，单击要管理的工具链，然后单击**管理**。或者，在应用程序“概述”页面的“持续交付”磁贴上，单击**查看工具链**，然后单击**管理**。  
1. 单击指向组织的链接。 
1. 在“管理组织”页面上，单击**邀请用户**，并输入用户的电子邮件地址。
1. 如果您想要为用户授予高级许可权，请选择**管理员**、**记帐管理员**或**审计员**复选框中的一个或多个。
1. 单击**邀请**。
1. 单击**保存**。

## 删除工具链
{: #deleting_a_toolchain}

您可以删除工具链，并指定您要删除的相关联工具集成。

1. 在 DevOps 仪表板的**工具链**选项卡上，单击要删除的工具链，然后单击**管理**。或者，在应用程序“概述”页面的“持续交付”磁贴上，单击**查看工具链**，然后单击**管理**。
1. 单击**删除工具链**，并复查要删除的工具集成。
1. 通过输入工具链的名称并单击**删除**，以确认删除。

 **提示**：当您删除 GitHub 工具集成时，不会从 GitHub 删除相关联的 GitHub 存储库。您必须手动从 GitHub 除去存储库。
