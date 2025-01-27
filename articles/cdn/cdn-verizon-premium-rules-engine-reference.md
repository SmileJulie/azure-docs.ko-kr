---
title: Azure CDN 규칙 엔진 참조 | Microsoft Docs
description: Azure CDN 규칙 엔진 일치 조건 및 기능에 대한 참조 설명서
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: 5fc611af75a7f733576f9343a4375fb56cacc030
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593159"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-reference"></a>Azure CDN에서 Verizon 프리미엄의 규칙 엔진 참조

이 아티클에서는 Azure CDN(Content Delivery Network) [규칙 엔진](cdn-verizon-premium-rules-engine.md)에 제공되는 일치 조건 및 기능에 대해 자세히 설명합니다.

규칙 엔진은 특정 형식의 요청이 CDN에서 처리되는 방식을 최종적으로 결정하도록 설계되었습니다.

**일반적인 사용**:

- 사용자 지정 캐시 정책을 재정의하거나 정의합니다.
- 중요한 콘텐츠에 대한 요청에 대해 보안을 유지하거나 거부합니다.
- 요청을 리디렉션합니다.
- 사용자 지정 로그 데이터를 저장합니다.

## <a name="terminology"></a>용어

규칙은 [**조건식**](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md), [**일치 조건**](cdn-verizon-premium-rules-engine-reference-match-conditions.md) 및 [**기능**](cdn-verizon-premium-rules-engine-reference-features.md)을 통해 정의됩니다. 이러한 요소는 다음 그림에 강조 표시되어 있습니다.

 ![CDN 일치 조건](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## <a name="syntax"></a>구문

특수 문자가 처리되는 방식은 일치 조건 또는 기능이 텍스트 값을 처리하는 방식에 따라 다릅니다. 일치 조건 또는 기능은 다음 방법 중 하나로 텍스트를 해석할 수 있습니다.

1. [**리터럴 값**](#literal-values)
2. [**와일드카드 값**](#wildcard-values)
3. [**정규식**](#regular-expressions)

### <a name="literal-values"></a>리터럴 값

리터럴 값으로 해석되는 텍스트는 % 기호를 제외한 모든 특수 문자를 일치되어야 하는 값의 일부로 취급합니다. 즉, `\'*'\`로 설정된 리터럴 일치 조건은 정확한 값(예: `\'*'\`)을 찾은 경우에만 충족됩니다.

백분율 기호는 URL 인코딩을 나타내는 데 사용됩니다(예: `%20`).

### <a name="wildcard-values"></a>와일드카드 값

와일드카드 값으로 해석되는 텍스트는 특수 문자에 추가적인 의미를 할당합니다. 다음 표에서는 다음 문자 집합이 해석되는 방식을 설명합니다.

문자 | Description
----------|------------
\ | 백슬래시는 이 테이블에 지정된 문자를 이스케이프하는 데 사용됩니다. 백슬래시는 이스케이프해야 하는 특수 문자 바로 앞에 지정되어야 합니다.<br/>예를 들어 다음 구문은 별표를 이스케이프합니다.`\*`
% | 백분율 기호는 URL 인코딩을 나타내는 데 사용됩니다(예: `%20`).
\* | 별표는 하나 이상의 문자를 나타내는 와일드카드입니다.
Space | 공백 문자는 지정된 값 또는 패턴에 의해 일치 조건이 충족될 수 있음을 나타냅니다.
'value' | 작은따옴표는 특별한 의미가 없습니다. 그러나 값을 리터럴 값으로 취급해야 함을 나타내기 위해 작은따옴표 쌍을 사용합니다. 다음과 같은 방법으로 사용할 수 있습니다.<br><br/>- 지정된 값이 비교 값의 일부와 일치할 때마다 일치 조건이 충족될 수 있습니다.  예를 들어 `'ma'`는 다음 문자열 중 하나와 일치합니다. <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/business/template.**ma**p<br /><br />- 특수 문자를 리터럴 문자로 지정할 수 있습니다. 예를 들어 공백 문자를 작은따옴표 쌍으로 묶어 리터럴 공백 문자를 지정할 수 있습니다(즉, `' '` 또는 `'sample value'`).<br/>- 빈 값을 지정할 수 있습니다. 작은따옴표 쌍(예: '')을 지정하여 빈 값을 지정합니다.<br /><br/>**중요:**<br/>- 지정된 값이 와일드 카드를 포함하지 않으면 자동으로 리터럴 값으로 간주됩니다. 즉, 작은따옴표 집합을 지정할 필요가 없습니다.<br/>- 백슬래시가 이 표의 다른 문자를 이스케이프하지 않으면 작은따옴표 집합으로 묶어서 지정할 때 무시됩니다.<br/>- 특수 문자를 리터럴 문자로 지정하는 또 다른 방법은 백슬래시를 사용하여 이스케이프하는 것입니다(즉, `\`).

### <a name="regular-expressions"></a>정규식

정규식은 텍스트 값 내에서 검색될 패턴을 정의합니다. 정규식 표기법은 다양한 기호에 대한 특정 의미를 정의합니다. 다음 표에서는 특수 문자가 정규식을 지원하는 일치 조건 및 기능에 의해 처리되는 방식을 나타냅니다.

특수 문자 | Description
------------------|------------
\ | 백슬래시는 뒤에 오는 문자를 이스케이프합니다. 그러면 해당 문자가 정규식 의미를 갖지 않고 리터럴 값으로 처리됩니다. 예를 들어 다음 구문은 별표를 이스케이프합니다.`\*`
% | The meaning of a percentage symbol depends on its usage.<br/><br/> `%{HTTPVariable}`: This syntax identifies an HTTP variable.<br/>`%{HTTPVariable%Pattern}`: This syntax uses a percentage symbol to identify an HTTP variable and as a delimiter.<br />`\%`: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\%20`).
\* | An asterisk allows the preceding character to be matched zero or more times.
Space | A space character is typically treated as a literal character.
'value' | Single quotes are treated as literal characters. A set of single quotes does not have special meaning.

## Next steps

- [Rules engine match conditions](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [Rules engine conditional expressions](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Rules engine features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Override HTTP behavior using the rules engine](cdn-verizon-premium-rules-engine.md)
- [Azure CDN overview](cdn-overview.md백분율 기호의 의미는 사용법에 따라 달라집니다. Docs
descr`%{HTTPVariable}`: 이 구문은 HTTP 변수를 식별 합니다.match`%{HTTPVariable%Pattern}`: 이 구문은 HTTP 변수 및 구분 기호로 식별 하는 백분율 기호를 사용 합니다.1/2019`\``: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\`20`).f the available match conditions and features for the Azure Content Delivery Network (CDN) [rules engine](cdn-verizon-premium-rules-engine.md).

The rules engine is designed to be the final authority on how specific types of requests are processed by the CDN.

**Common uses**:

- Override or define a custom cache policy.
- Secure or deny requests for sensitive content.
- Redirect requests.
- Store custom log data.

## Terminology

A rule is defined through the use of [**conditional expressions**](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md), [**match conditions**](cdn-verizon-premium-rules-engine-reference-match-conditions.md), and [**features**](cdn-verizon-premium-rules-engine-reference-features.md). These elements are highlighted in the following illustration:

 ![CDN match condition](./media/cdn-rules-engine-reference/cdn-rules-engine-terminology.png)

## Syntax

The manner in which special characters are treated varies according to how a match condition or feature handles text values. A match condition or feature may interpret text in one of the following ways:

1. [**Literal values**](#literal-values)
2. [**Wildcard values**](#wildcard-values)
3. [**Regular expressions**](#regular-expressions)

### Literal values

Text that is interpreted as a literal value treats all special characters, with the exception of the % symbol, as a part of the value that must be matched. In other words, a literal match condition set to `\'*'\` is only satisfied when that exact value (that is, `\'*'\`) is found.

A percentage symbol is used to indicate URL encoding (for example, `%20`).

### Wildcard values

Text that is interpreted as a wildcard value assigns additional meaning to special characters. The following table describes how the following set of characters is interpreted:

Character | Description
----------|------------
\ | A backslash is used to escape any of the characters specified in this table. A backslash must be specified directly before the special character that should be escaped.<br/>For example, the following syntax escapes an asterisk: `\*`
% | A percentage symbol is used to indicate URL encoding (for example, `%20`).
\* | An asterisk is a wildcard that represents one or more characters.
Space | A space character indicates that a match condition may be satisfied by either of the specified values or patterns.
'value' | A single quote does not have special meaning. However, a set of single quotes is used to indicate that a value should be treated as a literal value. It can be used in the following ways:<br><br/>- It allows a match condition to be satisfied whenever the specified value matches any portion of the comparison value.  For example, `'ma'` would match any of the following strings: <br/><br/>/business/**ma**rathon/asset.htm<br/>**ma**p.gif<br/>/business/template.**ma**p<br /><br />- It allows a special character to be specified as a literal character. For example, you may specify a literal space character by enclosing a space character within a set of single quotes (that is, `' '` or `'sample value'`).<br/>- It allows a blank value to be specified. Specify a blank value by specifying a set of single quotes (that is, '').<br /><br/>**Important:**<br/>- If the specified value does not contain a wildcard, then it is automatically considered a literal value, which means that it is not necessary to specify a set of single quotes.<br/>- If a backslash does not escape another character in this table, it is ignored when it is specified within a set of single quotes.<br/>- Another way to specify a special character as a literal character is to escape it using a backslash (that is, `\`).

### Regular expressions

Regular expressions define a pattern that is searched for within a text value. Regular expression notation defines specific meanings to a variety of symbols. The following table indicates how special characters are treated by match conditions and features that support regular expressions.

Special Character | Description
------------------|------------
\ | A backslash escapes the character the follows it, which causes that character to be treated as a literal value instead of taking on its regular expression meaning. For example, the following syntax escapes an asterisk: `\*`
% | The meaning of a percentage symbol depends on its usage.<br/><br/> `%{HTTPVariable}`: This syntax identifies an HTTP variable.<br/>`%{HTTPVariable%Pattern}`: This syntax uses a percentage symbol to identify an HTTP variable and as a delimiter.<br />`\%`: Escaping a percentage symbol allows it to be used as a literal value or to indicate URL encoding (for example, `\%20`).
\* | 별표를 사용하면 앞에 오는 문자의 일치 여부가 0번 이상 확인될 수 있습니다.
공백 | 공백 문자는 일반적으로 리터럴 문자로 취급됩니다.
'value' | 작은따옴표는 리터럴 문자로 처리됩니다. 작은따옴표 쌍은 특별한 의미가 없습니다.

## <a name="next-steps"></a>다음 단계

- [규칙 엔진 일치 조건](cdn-verizon-premium-rules-engine-reference-match-conditions.md)
- [규칙 엔진 조건식](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [규칙 엔진 기능](cdn-verizon-premium-rules-engine-reference-features.md)
- [규칙 엔진을 사용하여 HTTP 동작 재정의](cdn-verizon-premium-rules-engine.md)
- [Azure CDN 개요](cdn-overview.md)