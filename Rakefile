# frozen_string_literal: true

require 'digest'
require 'yaml'

Poem = Struct.new(:题目, :朝代, :作者, :诗词, :脚注, :诗注, :小传, :标签) do
  def self.from_yaml_file(file)
    yaml = YAML.safe_load_file(file)
    new(
      yaml['题目'], yaml['朝代'], yaml['作者'],
      yaml['诗词'] || [], yaml['脚注'] || [], yaml['诗注'], yaml['小传'], yaml['标签'] || []
    )
  end

  def initialize(...)
    super

    preprocess_footnotes

    self.诗词 = 诗词.join('<br>')
    self.脚注 = "<ol><li>#{脚注.join('</li><li>')}</li></ol>" unless 脚注.empty?
    self.标签 = 标签.join(' ')
  end

  def preprocess_footnotes
    pattern = /([[:alpha:]])\(([a-zāáǎàēéěèīíǐìōóǒòūúǔùüǖǘǚǜ]{1,5})\)/
    self.脚注 = 脚注.map { _1.gsub(pattern, '<ruby>\1<rp>(</rp><rt>\2</rt><rp>)</rp></ruby>') }
  end

  def to_text_line
    [
      题目, 朝代, 作者, 诗词, 脚注, 诗注, 小传, 标签,
      Digest::MD5.hexdigest([题目, 朝代, 作者].join("\t"))
    ].join("\t")
  end
end

task :default do
  fout = File.open('chinese-poetry.txt', 'w')

  # Add headers
  fout.puts <<~END_OF_DOC
    #separator:Tab
    #html:true
    #columns:#{%w[题目 朝代 作者 诗词 脚注 诗注 小传].join("\t")}
    #tags column:8
    #guid column:9
  END_OF_DOC

  # Add poems
  Dir['*/*.yml'].each { fout.puts Poem.from_yaml_file(_1).to_text_line }

  fout.close
end
