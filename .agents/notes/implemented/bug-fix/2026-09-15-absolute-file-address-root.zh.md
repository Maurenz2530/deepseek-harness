# Agent Note: Preserve the absolute POSIX root in file addresses

Status: implemented

[English](2026-09-15-absolute-file-address-root.md) | 中文

## Problem

`absoluteFileAddress('/')` 会生成 `dsh-resource://file/absolute/`，但 `parseFileAddress` 会把该地址当作缺少路径而拒绝。因此这个有效的 POSIX 绝对根目录无法往返转换。

## Decision

解析器将 `absolute/` 作用域后的空路径视为 POSIX 根目录。现有的 `absolute//` 表示仍保留给空主机名的 UNC 路径，并继续拒绝。

## Alternatives considered

**将根目录编码为非空的虚构路径段。** 这会保留对 `absolute/` 的旧拒绝行为，但会为已经能明确表示 POSIX 调用方路径的形式增加特殊线缆拼写。

## Consequences

POSIX 绝对根目录地址现在可以往返转换，消费者也能将其与格式错误的 UNC 地址区分开来。不带 Session 的地址仍不会授权文件读取；消费者继续使用现有的工作区和 Session 策略。
