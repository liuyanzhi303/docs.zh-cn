---
title: 创建和使用具有强名称的程序集
ms.date: 08/19/2019
helpviewer_keywords:
- strong-name bypass feature
- strong-named assemblies, about strong-named assemblies
- strong-named assemblies
- signing assemblies
- assemblies [.NET Framework], signing
- strong-named assemblies, scenarios
- assemblies [.NET Framework], strong-named
- strong-named assemblies, loading into trusted application domains
- assembly binding, strong-named
ms.assetid: ffbf6d9e-4a88-4a8a-9645-4ce0ee1ee5f9
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 55869c107d245738df3af5ca9bb1b22195e90024
ms.sourcegitcommit: 7b1ce327e8c84f115f007be4728d29a89efe11ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/13/2019
ms.locfileid: "70972773"
---
# <a name="create-and-use-strong-named-assemblies"></a><span data-ttu-id="641d4-102">创建和使用具有强名称的程序集</span><span class="sxs-lookup"><span data-stu-id="641d4-102">Create and use strong-named assemblies</span></span>

<span data-ttu-id="641d4-103">强名称是由程序集的标识加上公钥和数字签名组成的。其中，程序集的标识包括简单文本名称、版本号和区域性信息（如果提供的话）。</span><span class="sxs-lookup"><span data-stu-id="641d4-103">A strong name consists of the assembly's identity—its simple text name, version number, and culture information (if provided)—plus a public key and a digital signature.</span></span> <span data-ttu-id="641d4-104">它使用相应私钥从程序集文件生成。</span><span class="sxs-lookup"><span data-stu-id="641d4-104">It is generated from an assembly file using the corresponding private key.</span></span> <span data-ttu-id="641d4-105">（程序集文件包含程序集清单，该清单包含组成程序集的所有文件的名称和哈希。）</span><span class="sxs-lookup"><span data-stu-id="641d4-105">(The assembly file contains the assembly manifest, which contains the names and hashes of all the files that make up the assembly.)</span></span>

> [!WARNING]
> <span data-ttu-id="641d4-106">不要依赖于通过强名称实现安全性。</span><span class="sxs-lookup"><span data-stu-id="641d4-106">Do not rely on strong names for security.</span></span> <span data-ttu-id="641d4-107">它们仅提供唯一的标识。</span><span class="sxs-lookup"><span data-stu-id="641d4-107">They provide a unique identity only.</span></span>

<span data-ttu-id="641d4-108">具有强名称的程序集只能使用其他具有强名称的程序集的类型。</span><span class="sxs-lookup"><span data-stu-id="641d4-108">A strong-named assembly can only use types from other strong-named assemblies.</span></span> <span data-ttu-id="641d4-109">否则，将会危害强名称程序集的完整性。</span><span class="sxs-lookup"><span data-stu-id="641d4-109">Otherwise, the integrity of the strong-named assembly would be compromised.</span></span>

> [!NOTE]
> <span data-ttu-id="641d4-110">虽然 .NET Core 支持强名称程序集，而且 .NET Core 库中的所有程序集均已签名，但大多数第三方程序集不需要强名称。</span><span class="sxs-lookup"><span data-stu-id="641d4-110">Although .NET Core supports strong-named assemblies, and all assemblies in the .NET Core library are signed, the majority of third-party assemblies do not need strong names.</span></span> <span data-ttu-id="641d4-111">有关详细信息，请参阅 GitHub 上的[强名称签名](https://github.com/dotnet/corefx/blob/master/Documentation/project-docs/strong-name-signing.md)。</span><span class="sxs-lookup"><span data-stu-id="641d4-111">For more information, see [Strong Name Signing](https://github.com/dotnet/corefx/blob/master/Documentation/project-docs/strong-name-signing.md) on GitHub.</span></span>

## <a name="strong-name-scenario"></a><span data-ttu-id="641d4-112">强名称方案</span><span class="sxs-lookup"><span data-stu-id="641d4-112">Strong name scenario</span></span>

<span data-ttu-id="641d4-113">下面的方案概述了对具有强名称的程序集进行签名以及稍后使用该名称对其进行引用的过程。</span><span class="sxs-lookup"><span data-stu-id="641d4-113">The following scenario outlines the process of signing an assembly with a strong name and later referencing it by that name.</span></span>

1. <span data-ttu-id="641d4-114">使用以下方法之一创建具有强名称的程序集 A：</span><span class="sxs-lookup"><span data-stu-id="641d4-114">Assembly A is created with a strong name using one of the following methods:</span></span>

    - <span data-ttu-id="641d4-115">使用支持创建强名称的开发环境，如 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="641d4-115">Using a development environment that supports creating strong names, such as Visual Studio.</span></span>

    - <span data-ttu-id="641d4-116">使用[强名称工具 (Sn.exe)](../../framework/tools/sn-exe-strong-name-tool.md) 创建加密密钥对，并使用命令行编译器或[程序集链接器 (Al.exe)](../../framework/tools/al-exe-assembly-linker.md) 将密钥对分配到程序集。</span><span class="sxs-lookup"><span data-stu-id="641d4-116">Creating a cryptographic key pair using the [Strong Name tool (Sn.exe)](../../framework/tools/sn-exe-strong-name-tool.md) and assigning that key pair to the assembly using either a command-line compiler or the [Assembly Linker (Al.exe)](../../framework/tools/al-exe-assembly-linker.md).</span></span> <span data-ttu-id="641d4-117">Windows SDK 同时提供 Sn.exe 和 Al.exe。</span><span class="sxs-lookup"><span data-stu-id="641d4-117">The Windows SDK provides both Sn.exe and Al.exe.</span></span>

2. <span data-ttu-id="641d4-118">开发环境或工具对包含具有开发人员私钥的程序集清单的文件的哈希进行签名。</span><span class="sxs-lookup"><span data-stu-id="641d4-118">The development environment or tool signs the hash of the file containing the assembly's manifest with the developer's private key.</span></span> <span data-ttu-id="641d4-119">此数字签名存储在包含程序集 A 的清单的可移植可执行 (PE) 文件中。</span><span class="sxs-lookup"><span data-stu-id="641d4-119">This digital signature is stored in the portable executable (PE) file that contains Assembly A's manifest.</span></span>

3. <span data-ttu-id="641d4-120">程序集 B 是程序集 A 的一个使用者。程序集 B 清单的引用部分包含表示程序集 A 公钥的标记。</span><span class="sxs-lookup"><span data-stu-id="641d4-120">Assembly B is a consumer of Assembly A. The reference section of Assembly B's manifest includes a token that represents Assembly A's public key.</span></span> <span data-ttu-id="641d4-121">标记是完整公钥的一部分，并且使用它而不是密钥本身以节省空间。</span><span class="sxs-lookup"><span data-stu-id="641d4-121">A token is a portion of the full public key and is used rather than the key itself to save space.</span></span>

4. <span data-ttu-id="641d4-122">当程序集放置在全局程序集缓存中时，公共语言运行时验证强名称签名。</span><span class="sxs-lookup"><span data-stu-id="641d4-122">The common language runtime verifies the strong name signature when the assembly is placed in the global assembly cache.</span></span> <span data-ttu-id="641d4-123">在运行时按强名称绑定时，公共语言运行时会将存储在程序集 B 的清单中的密钥与为程序集 A 生成强名称的密钥进行比较。如果 .NET Framework 安全检查通过且绑定成功，程序集 B 就可保证程序集 A 的位未被篡改，且这些位确实来自程序集 A 的开发人员。</span><span class="sxs-lookup"><span data-stu-id="641d4-123">When binding by strong name at run time, the common language runtime compares the key stored in Assembly B's manifest with the key used to generate the strong name for Assembly A. If the .NET Framework security checks pass and the bind succeeds, Assembly B has a guarantee that Assembly A's bits have not been tampered with and that these bits actually come from the developers of Assembly A.</span></span>

> [!NOTE]
> <span data-ttu-id="641d4-124">此方案不解决信任问题。</span><span class="sxs-lookup"><span data-stu-id="641d4-124">This scenario doesn't address trust issues.</span></span> <span data-ttu-id="641d4-125">除强名称外，程序集可携带完整的 Microsoft Authenticode 签名。</span><span class="sxs-lookup"><span data-stu-id="641d4-125">Assemblies can carry full Microsoft Authenticode signatures in addition to a strong name.</span></span> <span data-ttu-id="641d4-126">Authenticode 签名包括建立信任的证书。</span><span class="sxs-lookup"><span data-stu-id="641d4-126">Authenticode signatures include a certificate that establishes trust.</span></span> <span data-ttu-id="641d4-127">请务必注意强名称不要求代码以这种方式进行签名。</span><span class="sxs-lookup"><span data-stu-id="641d4-127">It's important to note that strong names don't require code to be signed in this way.</span></span> <span data-ttu-id="641d4-128">强名称仅提供唯一的标识。</span><span class="sxs-lookup"><span data-stu-id="641d4-128">Strong names only provide a unique identity.</span></span>

## <a name="bypass-signature-verification-of-trusted-assemblies"></a><span data-ttu-id="641d4-129">跳过受信任程序集的签名验证</span><span class="sxs-lookup"><span data-stu-id="641d4-129">Bypass signature verification of trusted assemblies</span></span>

<span data-ttu-id="641d4-130">从 .NET Framework 3.5 Service Pack 1 开始，当程序集加载到完全信任的应用程序域（如 `MyComputer` 区域的默认应用程序域）时，不会验证强名称签名。</span><span class="sxs-lookup"><span data-stu-id="641d4-130">Starting with the .NET Framework 3.5 Service Pack 1, strong-name signatures are not validated when an assembly is loaded into a full-trust application domain, such as the default application domain for the `MyComputer` zone.</span></span> <span data-ttu-id="641d4-131">这被称之为强名称跳过功能。</span><span class="sxs-lookup"><span data-stu-id="641d4-131">This is referred to as the strong-name bypass feature.</span></span> <span data-ttu-id="641d4-132">在完全信任的环境中，对于已签名的完全信任的程序集，对 <xref:System.Security.Permissions.StrongNameIdentityPermission> 的需求总是成功，而不考虑其签名。</span><span class="sxs-lookup"><span data-stu-id="641d4-132">In a full-trust environment, demands for <xref:System.Security.Permissions.StrongNameIdentityPermission> always succeed for signed, full-trust assemblies, regardless of their signature.</span></span> <span data-ttu-id="641d4-133">这种情况下，强名称跳过功能可避免完全信任程序集不必要的强名称签名验证开销，允许更快地加载程序集。</span><span class="sxs-lookup"><span data-stu-id="641d4-133">The strong-name bypass feature avoids the unnecessary overhead of strong-name signature verification of full-trust assemblies in this situation, allowing the assemblies to load faster.</span></span>

<span data-ttu-id="641d4-134">跳过功能适用于使用强名称进行签名及具有以下特征的任何程序集：</span><span class="sxs-lookup"><span data-stu-id="641d4-134">The bypass feature applies to any assembly that is signed with a strong name and that has the following characteristics:</span></span>

- <span data-ttu-id="641d4-135">完全受信任，无需 <xref:System.Security.Policy.StrongName> 证据（如具有 `MyComputer` 区域证据）。</span><span class="sxs-lookup"><span data-stu-id="641d4-135">Fully trusted without <xref:System.Security.Policy.StrongName> evidence (for example, has `MyComputer` zone evidence).</span></span>

- <span data-ttu-id="641d4-136">加载到完全受信任的 <xref:System.AppDomain>。</span><span class="sxs-lookup"><span data-stu-id="641d4-136">Loaded into a fully trusted <xref:System.AppDomain>.</span></span>

- <span data-ttu-id="641d4-137">加载自该 <xref:System.AppDomain> 的 <xref:System.AppDomainSetup.ApplicationBase%2A> 属性下的某个位置。</span><span class="sxs-lookup"><span data-stu-id="641d4-137">Loaded from a location under the <xref:System.AppDomainSetup.ApplicationBase%2A> property of that <xref:System.AppDomain>.</span></span>

- <span data-ttu-id="641d4-138">签名没有延迟。</span><span class="sxs-lookup"><span data-stu-id="641d4-138">Not delay-signed.</span></span>

<span data-ttu-id="641d4-139">可为单个应用程序或计算机禁用此功能。</span><span class="sxs-lookup"><span data-stu-id="641d4-139">This feature can be disabled for individual applications or for a computer.</span></span> <span data-ttu-id="641d4-140">请参阅[如何：禁用强名称跳过功能](disable-strong-name-bypass-feature.md)。</span><span class="sxs-lookup"><span data-stu-id="641d4-140">See [How to: Disable the strong-name bypass feature](disable-strong-name-bypass-feature.md).</span></span>

## <a name="related-topics"></a><span data-ttu-id="641d4-141">相关主题</span><span class="sxs-lookup"><span data-stu-id="641d4-141">Related topics</span></span>

|<span data-ttu-id="641d4-142">Title</span><span class="sxs-lookup"><span data-stu-id="641d4-142">Title</span></span>|<span data-ttu-id="641d4-143">说明</span><span class="sxs-lookup"><span data-stu-id="641d4-143">Description</span></span>|
|-----------|-----------------|
|[<span data-ttu-id="641d4-144">如何：创建公钥/私钥对</span><span class="sxs-lookup"><span data-stu-id="641d4-144">How to: Create a public-private key pair</span></span>](create-public-private-key-pair.md)|<span data-ttu-id="641d4-145">描述如何创建加密密钥对以对程序集进行签名。</span><span class="sxs-lookup"><span data-stu-id="641d4-145">Describes how to create a cryptographic key pair for signing an assembly.</span></span>|
|[<span data-ttu-id="641d4-146">如何：使用强名称为程序集签名</span><span class="sxs-lookup"><span data-stu-id="641d4-146">How to: Sign an assembly with a strong name</span></span>](sign-strong-name.md)|<span data-ttu-id="641d4-147">介绍如何创建具有强名称的程序集。</span><span class="sxs-lookup"><span data-stu-id="641d4-147">Describes how to create a strong-named assembly.</span></span>|
|[<span data-ttu-id="641d4-148">改进的强命名</span><span class="sxs-lookup"><span data-stu-id="641d4-148">Enhanced strong naming</span></span>](enhanced-strong-naming.md)|<span data-ttu-id="641d4-149">描述 .NET Framework 4.5 中强名称的改进。</span><span class="sxs-lookup"><span data-stu-id="641d4-149">Describes enhancements to strong-names in the .NET Framework 4.5.</span></span>|
|[<span data-ttu-id="641d4-150">如何：引用具有强名称的程序集</span><span class="sxs-lookup"><span data-stu-id="641d4-150">How to: Reference a strong-named assembly</span></span>](reference-strong-named.md)|<span data-ttu-id="641d4-151">介绍如何在编译时或运行时引用具有强名称的程序集中的类型或资源。</span><span class="sxs-lookup"><span data-stu-id="641d4-151">Describes how to reference types or resources in a strong-named assembly at compile time or run time.</span></span>|
|[<span data-ttu-id="641d4-152">如何：禁用强名称跳过功能</span><span class="sxs-lookup"><span data-stu-id="641d4-152">How to: Disable the strong-name bypass feature</span></span>](disable-strong-name-bypass-feature.md)|<span data-ttu-id="641d4-153">描述如何禁用跳过强名称签名验证的功能。</span><span class="sxs-lookup"><span data-stu-id="641d4-153">Describes how to disable the feature that bypasses the validation of strong-name signatures.</span></span> <span data-ttu-id="641d4-154">可为所有或特定应用程序禁用此功能。</span><span class="sxs-lookup"><span data-stu-id="641d4-154">This feature can be disabled for all or for specific applications.</span></span>|
|[<span data-ttu-id="641d4-155">创建程序集</span><span class="sxs-lookup"><span data-stu-id="641d4-155">Create assemblies</span></span>](create.md)|<span data-ttu-id="641d4-156">提供单个文件和多文件程序集的概述。</span><span class="sxs-lookup"><span data-stu-id="641d4-156">Provides an overview of single-file and multifile assemblies.</span></span>|
|[<span data-ttu-id="641d4-157">如何在 Visual Studio 中延迟对程序集的签名</span><span class="sxs-lookup"><span data-stu-id="641d4-157">How to delay sign an assembly in Visual Studio</span></span>](/visualstudio/ide/managing-assembly-and-manifest-signing#how-to-sign-an-assembly-in-visual-studio)|<span data-ttu-id="641d4-158">说明如何在创建程序集后对具有强名称的程序集进行签名。</span><span class="sxs-lookup"><span data-stu-id="641d4-158">Explains how to sign an assembly with a strong name after the assembly has been created.</span></span>|
|[<span data-ttu-id="641d4-159">Sn.exe（强名称工具）</span><span class="sxs-lookup"><span data-stu-id="641d4-159">Sn.exe (Strong Name tool)</span></span>](../../framework/tools/sn-exe-strong-name-tool.md)|<span data-ttu-id="641d4-160">介绍 .NET Framework 中包含的可帮助创建具有强名称的程序集的工具。</span><span class="sxs-lookup"><span data-stu-id="641d4-160">Describes the tool included in the .NET Framework that helps create assemblies with strong names.</span></span> <span data-ttu-id="641d4-161">此工具提供有关密钥管理、签名生成和签名验证的选项。</span><span class="sxs-lookup"><span data-stu-id="641d4-161">This tool provides options for key management, signature generation, and signature verification.</span></span>|
|[<span data-ttu-id="641d4-162">Al.exe（程序集链接器）</span><span class="sxs-lookup"><span data-stu-id="641d4-162">Al.exe (Assembly linker)</span></span>](../../framework/tools/al-exe-assembly-linker.md)|<span data-ttu-id="641d4-163">介绍 .NET Framework 中包含的一种工具，该工具可生成具有模块或资源文件的程序集清单的文件。</span><span class="sxs-lookup"><span data-stu-id="641d4-163">Describes the tool included in the .NET Framework that generates a file that has an assembly manifest from modules or resource files.</span></span>|