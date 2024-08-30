---
layout: default
permalink: /appendix/
title: 付録
class: license-types
---

<budoux-ja>参考までに、</budoux-ja>[choosealicense.com リポジトリ](https://github.com/github/choosealicense.com)<budoux-ja>で説明されているすべてのライセンスの表を以下に示します。</budoux-ja>

<budoux-ja>ライセンスを選択するためにここにいる場合は、ほとんどの場合に適用されるいくつかのライセンスを表示するために、</budoux-ja>**[ホームページから始めてください](/)**。

<table border style="font-size: xx-small; position: relative">
{% assign types = "permissions|conditions|limitations" | split: "|" %}
<tr style="position: sticky; top: 0; z-index: 1000001; background: color-mix(in srgb, var(--backgroundColor) 70%, transparent);">
  <th scope="col" style="text-align: center">License</th>
  {% assign seen_tags = '' %}
  {% for type in types %}
    {% assign rules = site.data.rules[type] | sort: "label" %}
    {% for rule_obj in rules %}
      {% if seen_tags contains rule_obj.tag or rule_obj.tag contains '--' %}
        {% continue %}
      {% endif %}
      {% capture seen_tags %}{{ seen_tags | append:rule_obj.tag }}{% endcapture %}
      <th scope="col" style="text-align: center; width:7%"><a href="#{{ rule_obj.tag }}"><budoux-ja>{{ rule_obj.label_ja }}</budoux-ja></a></th>
    {% endfor %}
  {% endfor %}
</tr>
{% assign licenses = site.licenses | sort: "path" %}
{% for license in licenses %}
  <tr style="height: 3em"><th scope="row"><a href="{{ license.id }}">{{ license.title }}</a></th>
  {% assign seen_tags = '' %}
  {% for type in types %}
    {% assign rules = site.data.rules[type] | sort: "label" %}
    {% for rule_obj in rules %}
      {% assign req = rule_obj.tag %}
      {% if seen_tags contains req  or rule_obj.tag contains '--' %}
        {% continue %}
      {% endif %}
      {% capture seen_tags %}{{ seen_tags | append:req }}{% endcapture %}
      {% assign seen_req = false %}
      {% for t in types %}
        {% for r in license[t] %}
          {% if r contains req %}
            <td class="license-{{ t }}" style="text-align:center">
              {% if r contains "--" %}
                {% assign lite = " lite" %}
              {% else %}
                {% assign lite = "" %}
              {% endif %}
              <span class="{{ r | append: lite }}" style="margin: auto;">
                <span class="license-sprite {{ r }}"></span>
              </span>
            </td>
            {% assign seen_req = true %}
          {% endif %}
        {% endfor %}
      {% endfor %}
      {% unless seen_req %}
        <td></td>
      {% endunless %}
    {% endfor %}
  {% endfor %}
  </tr>
{% endfor %}
</table>

## 凡例

<p><budoux-ja>オープンソースライセンスは、著作権その他の「知的財産」法が許容しないことを許可する</budoux-ja><span class="license-permissions"><span class="license-sprite"></span></span> <b><budoux-ja>権限</budoux-ja></b><budoux-ja>を一般に提供します。</budoux-ja></p>

<p><budoux-ja>ほとんどのオープンソースライセンスの許可は、</budoux-ja><span class="license-conditions"><span class="license-sprite"></span></span> <b><budoux-ja>条件</budoux-ja></b><budoux-ja>の遵守に従うことが前提となっています。</budoux-ja></p>

<p><budoux-ja>ほとんどのオープンソースライセンスには、</budoux-ja><span class="license-limitations"><span class="license-sprite"></span></span> <b><budoux-ja>制限事項</budoux-ja></b><budoux-ja>があり、通常は保証や責任を否認し、特許や商標を明示的に除外することがあります。</budoux-ja></p>

<dl>
{% assign seen_tags = '' %}
{% for type in types %}
  {% assign rules = site.data.rules[type] | sort: "label" %}
  {% for rule_obj in rules %}
    {% assign req = rule_obj.tag %}
    {% if seen_tags contains req %}
      {% continue %}
    {% endif %}
    <dt id="{{ req }}">{{ rule_obj.label_ja }}<budoux-ja>（{{ rule_obj.label }}）</budoux-ja></dt>
    {% capture seen_tags %}{{ seen_tags | append:req }}{% endcapture %}
    {% for t in types %}
      {% assign rs = site.data.rules[t] | sort: "label" %}
      {% for r in rs %}
        {% if r.tag == req %}
          <dd class="license-{{t}}"><span class="license-sprite"></span><budoux-ja>{{ r.description }}</budoux-ja></dd>
        {% endif %}
      {% endfor %}
    {% endfor %}
  {% endfor %}
{% endfor %}
</dl>
