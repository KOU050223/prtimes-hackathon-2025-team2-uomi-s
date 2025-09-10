# Feature Specification: 記事の中の強みを分析

**Feature Branch**: `issue18`  
**Created**: 2025-09-09  
**Status**: Draft  
**Input**: User description: "issue18を達成する"

## Execution Flow (main)
```
1. Parse user description from Input
   → If empty: ERROR "No feature description provided"
2. Extract key concepts from description
   → Identify: actors, actions, data, constraints
3. For each unclear aspect:
   → Mark with [NEEDS CLARIFICATION: specific question]
4. Fill User Scenarios & Testing section
   → If no clear user flow: ERROR "Cannot determine user scenarios"
5. Generate Functional Requirements
   → Each requirement must be testable
   → Mark ambiguous requirements
6. Identify Key Entities (if data involved)
7. Run Review Checklist
   → If any [NEEDS CLARIFICATION]: WARN "Spec has uncertainties"
   → If implementation details found: ERROR "Remove tech details"
8. Return: SUCCESS (spec ready for planning)
```

---

## ⚡ Quick Guidelines
- ✅ Focus on WHAT users need and WHY
- ❌ Avoid HOW to implement (no tech stack, APIs, code structure)
- 👥 Written for business stakeholders, not developers

### Section Requirements
- **Mandatory sections**: Must be completed for every feature
- **Optional sections**: Include only when relevant to the feature
- When a section doesn't apply, remove it entirely (don't leave as "N/A")

### For AI Generation
When creating this spec from a user prompt:
1. **Mark all ambiguities**: Use [NEEDS CLARIFICATION: specific question] for any assumption you'd need to make
2. **Don't guess**: If the prompt doesn't specify something (e.g., "login system" without auth method), mark it
3. **Think like a tester**: Every vague requirement should fail the "testable and unambiguous" checklist item
4. **Common underspecified areas**:
   - User types and permissions
   - Data retention/deletion policies  
   - Performance targets and scale
   - Error handling behaviors
   - Integration requirements
   - Security/compliance needs

---

## User Scenarios & Testing *(mandatory)*

### Primary User Story
ユーザーは単一のMarkdown形式の記事ファイルをアップロードし、その中に含まれる企業や組織の強みを自動的に分析・抽出できる機能を必要としている。これにより、手動での分析時間を削減し、効率的に競合分析やマーケティング戦略立案を支援する。

### Acceptance Scenarios
1. **Given** ユーザーがMarkdown形式の記事ファイルを持っている, **When** システムに記事ファイルをアップロードする, **Then** システムが記事内の強みを分析し、構造化された形で強みのリストを表示する
2. **Given** 記事ファイルに複数のセクションや段落が含まれている, **When** 分析を実行する, **Then** システムが各セクション内の強みを識別し、位置情報と共に整理して表示する
3. **Given** 分析結果を他の用途に使用したい, **When** 分析が完了している, **Then** 結果をエクスポート可能な形式で出力できる

### 分析対象例（ダミーデータとメディアフック9要素）
PR TIMESハッカソンの記事から以下のような強みが抽出されることを想定：

**特級性/希少性 (Superlative/Rarity)**:
- 「年収500万円以上の中途採用基準での内定」→ 学生にとっての異例的高待遇
- 「累計200万件超のプレスリリースデータをAPIで提供」→ 大規模データの特級性
- 「利用企業数10万8000社以上」→ 市場シェアの絶対的数値
- 「上場企業の61％超が利用」→ 高い利用率の数値

**新規性/独自性 (Novelty/Uniqueness)**:
- 「チーム開発×データ分析に挑む3Daysハッカソン」→ 独特な領域組み合わせ
- 「プレスリリースを改善するレビュー機能」→ 新しいアプローチ

**時代性/季節性 (Time/Seasonality)**:
- 「2026・27年卒業予定のエンジニア志望学生が対象」→ タイムリーな採用時期
- 　2025年9月開催」→ 夏休みイベントの適切なタイミング

**社会性/公共性 (Social/Public Interest)**:
- 「交通費・宿泊費を会社負担」→ 学生への社会的サポート
- 「全国から学生エンジニアが集まる」→ 地方学生支援

**実績/信頼性**:
- 「2016年より開催している内定直結型」→ 長年の実績と直接的な成果

**強み不足の指摘例**:
- 「矛盾/対立 (Contradiction/Conflict)」要素が不足 → 一般的なハッカソンとの違いを明確化できる要素が必要
- 「意外性 (Unexpectedness)」要素が不足 → 驚きやインパクトのある要素が必要

**特に優れた強みのハイライト例**:
- 「年収500万円以上」→ インパクトスコア: 高（学生にとって異例的）
- 「累計200万件超」→ インパクトスコア: 高（大規模データの特級性）
- 「上場企業61％超が利用」→ インパクトスコア: 中（信頼性はあるが一般的）

### Edge Cases
- テキストに明確な強みが含まれていない場合はどのように処理するか？
- 強みが不足しているメディアフック要素がある場合、その指摘をどのように表示するか？
- 特に優れた強みと一般的な強みをどのように区別して提示するか？
- 非常に長いテキストや特殊な文字が含まれる場合の処理はどうするか？
- 分析処理中にエラーが発生した場合のユーザー体験はどうするか？

## Requirements *(mandatory)*

### Functional Requirements
- **FR-001**: システムは単一のMarkdown形式の記事ファイルをアップロード機能で受け取ることができなければならない
- **FR-002**: システムは入力されたテキストから企業や組織の強みを自動的に識別・抽出しなければならない
- **FR-003**: システムは抽出した強みを構造化されたデータとして整理・分類しなければならない
- **FR-004**: ユーザーは分析結果を視覚的に理解しやすい形で閲覧できなければならない
- **FR-005**: システムは分析処理の進行状況をユーザーに表示しなければならない
- **FR-006**: システムはJSON形式で強みの分析結果を出力できなければならない
- **FR-007**: システムは各強みに対してメディアフック9要素（時代性、特級性、新規性、社会性等）で分類しなければならない
- **FR-010**: システムは各強みに対してメディアフックの根拠URL（https://prtimes.jp/magazine/media-hook/）を参照情報として付与しなければならない
- **FR-008**: システムは元記事での強みの記載位置（見出し、段落番号等）を特定できなければならない
- **FR-009**: システムは定量的な情報（数値、期間等）と定性的な情報（特徴、評価等）を区別して抽出しなければならない
- **FR-011**: システムはメディアフック9要素の中で不足している要素を特定し、改善提案として指摘しなければならない
- **FR-012**: システムは特に優れた強み（高い数値、独自性、希少性等）をハイライトし、優先推奨として指定しなければならない
- **FR-013**: システムは各強みに対してインパクトスコア（低/中/高）を付与し、メディア訴求力を評価しなければならない

### Key Entities *(include if feature involves data)*
- **記事ファイル**: 分析対象となるMarkdown形式のコンテンツ、ファイル名、アップロード日時などの属性を含む
- **強み情報**: 抽出された強み項目、メディアフック9要素による分類、インパクトスコア（低/中/高）、定量・定性の区分、元テキストでの位置情報
- **改善提案**: 不足しているメディアフック要素、具体的な改善方法、推奨アクション
- **ハイライト**: 特に優れた強みリスト、インパクトスコア順のランキング、推奨アピールポイント
- **分析結果**: 記事と抽出された強みの関連付け、メディアフック9要素別の強み統計、分析実行日時、処理ステータス、根拠ソース情報

---

## Review & Acceptance Checklist
*GATE: Automated checks run during main() execution*

### Content Quality
- [ ] No implementation details (languages, frameworks, APIs)
- [ ] Focused on user value and business needs
- [ ] Written for non-technical stakeholders
- [ ] All mandatory sections completed

### Requirement Completeness
- [ ] No [NEEDS CLARIFICATION] markers remain
- [ ] Requirements are testable and unambiguous  
- [ ] Success criteria are measurable
- [ ] Scope is clearly bounded
- [ ] Dependencies and assumptions identified

---

## Execution Status
*Updated by main() during processing*

- [x] User description parsed
- [x] Key concepts extracted
- [x] Ambiguities marked
- [x] User scenarios defined
- [x] Requirements generated
- [x] Entities identified
- [ ] Review checklist passed

---