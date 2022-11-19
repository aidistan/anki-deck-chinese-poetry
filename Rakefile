require 'digest'
require 'yaml'

task :default do
  fout = File.open('chinese-poetry.txt', 'w')

  fout.puts <<~END_OF_DOC
    #separator:Tab
    #html:true
    #columns:#{%w[题目 朝代 作者 诗词 脚注 诗注 小传].join("\t")}
    #tags column:8
    #guid column:9
  END_OF_DOC

  Dir['*/*.yml'].each do |file|
    poem = YAML.load(File.read(file))

    fout.puts [
      poem['题目'], poem['朝代'], poem['作者'],
      poem['诗词'].join('<br>'),
      poem['脚注'] ? '<ol><li>' + poem['脚注'].join('</li><li>') + '</li></ol>' : '',
      poem['诗注'],
      poem['小传'],
      poem['标签'] ? poem['标签'].join(' ') : '',
      Digest::MD5.hexdigest([poem['题目'], poem['朝代'], poem['作者']].join("\t")),
    ].join("\t")
  end

  fout.close
end
